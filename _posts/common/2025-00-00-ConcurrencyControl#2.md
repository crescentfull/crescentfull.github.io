---
title: "#2 동시성 제어 그리고 Django"
description: "Django에서는 동시성 제어를 어떻게 해야할까?"
date: 2025-06-04 10:00:00 +0900
categories: [CS, Common]
tags: [CS, Common, Concurrency, Database, Multithreading, Transaction, Lock, Django]
---

# Django에서는 동시성(Concurrency)을 어떻게 제어할까?

## 1. 데이터베이스 동시성 제어 (DB Concurrency Control)
DJnago의 ORM을 활용한 데이터베이스 락(Lock) 방식을 주로 사용한다.

- **select_for_update()**: 데이터 조회 시 행 단위의 락을 걸어, 특정 데이터를 다른 트랜잭션이 수정하지 못하도록 제어. 즉, 트랜잭션 종료 시까지 다른 트랜잭션의 접근을 막는다. 
- 트랜잭션 범위(transaction.atomic) 내에서만 유효하며, 트랜잭션이 끝나면 자동으로 해제된다.

```python
from django.db import transaction
from shop.models import Inventory

# 재고 감소 처리 (트랜잭션 내 동시 접근 제한)
with transaction.atomic():
    inventory_item = Inventory.objects.select_for_update().get(product_id=1001)
    if inventory_item.quantity > 0:
        inventory_item.quantity -= 1
        inventory_item.save()

이 코드가 실행 되는 동안 동일한 상품의 재고 데이터는 다른 트랜잭션에서 변경할 수 없으며, 변경이 완료된 후에야 다른 트랜잭션에서 접근이 가능하다.
```
- select_for_update()를 통해 다른 트랜잭션이 데이터를 읽거나 수정하는 것을 방지해 동시성 문제를 해결
- 주로 금융 거래나 재고 관리와 같은 민감한 데이터 처리에 적합

## 2. 낙관적 잠금(Optimistic Locking)
낙관적 잠금은 데이터 충돌이 자주 발생하지 않는다고 가정하여 미리 락을 걸지 않고 변경 시점에서 충돌 여부를 확인하는 방식이다.  
- 일반적으로 별도의 버전 필드를 둬서 관리
- 데이터를 업데이트할 때, 현재 버전을 확인한 뒤 업데이트를 시도하고, 버전이 다르면 충돌로 간주하여 처리.

```python
# 모델 정의
class Article(models.Model):
    content = models.TextField()
    version = models.PositiveIntegerField(default=0)

# 업데이트 시도
def update_post(post_id, new_content, current_version):
    rows_updated = Post.objects.filter(id=post_id, version=current_version)
                                .update(content=new_content, version=current_version+1)
    if row_updated == 0:
        raise Exception("충돌 발생")

>>> 업데이트 시, 버전 체크를 통해 충돌이 있으면 업데이트 되지 않는다.!       
```

 **장점**: 경쟁상태가 낮은 환경에서 효율적이며 DB 락으로 인한 성능 부하가 적다.
 **단점**: 충돌 시 재시도 로직을 별도로 구현해야 하며 충돌 빈도가 높다면 효율이 떨어진다.

## 3. 비관적 잠금(Pessimistic Locking)  
비관적 잠금은 데이터 접근 시 항상 락을 걸어 충돌 가능성을 아예 방지하는 방식이다.

- 데이터를 읽을 때부터 락을 걸어 다른 트랜잭션의 접근을 막는다.
- 즉, 락이 선재적으로 걸린 상태에서 작업을 수행하므로 데이터 충돌 자체가 발생하지 않는 것이다.

```python

from django.db import transaction
from myapp.models import Account

with transaction.atomic():
    account = Account.objects.select_for_update().get(user_id=42)
    account.balance -= 100
    account.save()


```
**
**장점**: 강력한 데이터 무결성을 보장한다. 데이터 손상을 예방하는 가장 확실한 방법이다.
**단점**: 과도한 락 사용은 성능 저하 및 데드락 위험을 높이며 관리가 까다롭다.

## 애플리케이션 수준의 동시성 제어 (Redis Lock)

데이터베이스 수준의 락은 트랜잭션 범위 내에서만 유효하므로, 여러 프로세스나 서버 간에 공유되는 자원(예: API 호출 빈도 제한, 배치 잡 수행 제어 등)을 컨트롤할 때는 애플리케이션 레벨의 분산 락(distributed lock)이 필요하다. 가장 보편적인 방법은 **Redis**를 락 서버로 활용하는 것이다.

### 4.1. Redis 분산 락 개요

* **목적**

  * 여러 Django 워커(process) 또는 서버 인스턴스가 동시에 동일한 로직을 수행하지 못하도록 제어
  * 예: cron 스케줄 잡이 이중 실행되는 것을 방지, 파일 업로드 후 후처리가 중복으로 일어나는 것을 방지
* **원리**

  1. 락 키(key)를 Redis에 `SETNX`(SET if Not eXists)로 등록
  2. 등록 성공 시 작업 수행, 실패 시 건너뛰거나 재시도
  3. TTL(만료 시간)을 함께 설정하여 락 홀딩 중 서비스 장애로 해제 못 할 경우 자동 해제

### 4.2. django-redis의 Lock API 사용 예제

```python
from django_redis import get_redis_connection

def perform_scheduled_task():
    redis = get_redis_connection("default")
    lock = redis.lock("myapp:scheduled_task_lock", timeout=60)  # 60초 후 자동 해제

    # 락 획득 시도 (최대 대기 10초)
    if lock.acquire(blocking=True, blocking_timeout=10):
        try:
            # 중복 실행을 막아야 하는 로직
            do_heavy_job()
        finally:
            lock.release()
    else:
        # 다른 프로세스가 이미 작업 중인 경우
        logger.info("Scheduled task is already running elsewhere.")
```

* `blocking` / `blocking_timeout` 옵션으로 대기/타임아웃 전략을 조정
* `timeout`을 적절히 설정해, 작업이 예상보다 길어져도 락이 자동 해제되도록 함

### 4.3. Redlock 알고리즘

Redlock은 Redis 클러스터(마스터 노드 복제 구조) 환경에서 안전한 분산 락을 제공하기 위해 제안된 알고리즘이다.

* **특징**

  1. 다수의 Redis 인스턴스(master) 중 과반수 이상에서 락 획득
  2. 클라이언트 시계 오차 및 네트워크 지연을 고려한 타임아웃
* **장점**: 단일 인스턴스 장애나 분할 뇌(split-brain) 상황에서도 비교적 안정적인 락 보장
* **단점**: 구현 복잡도 상승, 네트워크 RTT가 많아져 락 획득 대기 시간이 늘어날 수 있음

```python
import redlock

dlm = redlock.Redlock([
    {"host": "redis1.example.com", "port": 6379, "db": 0},
    {"host": "redis2.example.com", "port": 6379, "db": 0},
    {"host": "redis3.example.com", "port": 6379, "db": 0},
])

# 락 획득 (2000ms TTL)
lock = dlm.lock("resource_name", 2000)
if lock:
    try:
        # 분산 락이 보장된 작업
        critical_section()
    finally:
        dlm.unlock(lock)
else:
    # 락 획득 실패 시 로직
    handle_lock_failure()
```

---

## 5. 그 외 동시성 제어 기법

### 5.1. Celery를 활용한 태스크 큐

* **장점**: 작업이 백그라운드에서 비동기 실행되어, 한 번만 스케줄링할 수 있음
* **예**: `@singleton` 데코레이터로 태스크 이중 실행 방지

### 5.2. Django 3+ 비동기 뷰(async view)

* HTTP 레벨에서 I/O 바운드(외부 API 호출 등) 비동기 처리를 통해 동시 요청 처리량을 높임
* 주의: ORM은 여전히 동기 API이므로, DB 락과 함께 사용 시 deadlock 발생 가능

### 5.3. 로컬 스레드/프로세스 락

* `threading.Lock`, `multiprocessing.Lock`
* 단일 프로세스 내에서만 유효하며, Django의 기본 WSGI 서버 환경(다중 프로세스)에서는 분산 락이 필요

---


* **데이터 정합성**이 최우선이면 DB 수준의 `select_for_update()`
* **경합이 적은 경우** 낙관적 잠금을, **높은 무결성**이 필요하면 비관적 잠금을 선택
* **여러 인스턴스**에서 자원 접근을 제어해야 할 땐 Redis 분산 락(Redis Lock / Redlock)
* **백그라운드 잡**은 Celery 등 태스크 큐를 적극 활용

각 상황에 맞는 동시성 제어 기법을 적절히 조합하여, Django 애플리케이션의 안정성과 성능을 모두 잡아 보자!
