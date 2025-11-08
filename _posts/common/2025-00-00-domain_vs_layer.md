---
title: "Django 생태계 이해"
description: "django는 어떤 아키텍쳐 기반으로 만들어진 프레임워크일까"
date: 2025-11-07 10:00:00 +0900
categories: [CS, Common]
tags: [Django, DDD, Architecture]
---

Django는 **Python 기반의 웹 프레임워크**로, “웹 개발을 빠르고 안전하게 하기 위한 종합 세트”라고 할 수 있다.
한 줄로 정의하자면, **‘반복되는 웹 개발 과정을 자동화하여, 개발자가 서비스의 본질적인 문제 해결에 집중할 수 있도록 돕는 프레임워크’**다.

---

# Django 아키텍처 이해

### — MTV, Active Record, Domain Architecture, Clean Architecture

---

## 1. Django의 기원과 목적

Django는 Python 기반의 웹 프레임워크로,
“반복되는 웹 개발을 자동화하고 빠르게 개발할 수 있도록 돕는 도구”다.
2003년 미국의 지역 신문사 웹 개발팀에서 만들어졌으며,
**DRY(Don’t Repeat Yourself)** 원칙을 핵심 철학으로 삼았다.
즉, 같은 코드를 반복하지 않고 재사용 가능한 구조를 지향한다.

---

## 2. MTV 구조 (Model–Template–View)

Django는 MVC(Model–View–Controller) 패턴을 기반으로 하지만, 용어가 약간 다르다.
Django에서는 이를 **MTV(Model–Template–View)** 구조로 부른다.

* **Model**: 데이터 구조 정의 및 ORM 매핑
* **Template**: 사용자에게 보여줄 화면(HTML)을 정의
* **View**: 요청을 처리하고 Model과 Template을 연결

요청이 들어오면 View가 Model에서 데이터를 가져오고, Template에 담아 응답을 반환한다.
이 구조는 웹 요청–응답 흐름을 단순하고 일관성 있게 유지하도록 돕는다.

---

## 3. Django ORM과 Active Record 패턴

Django ORM(Object Relational Mapper)은 **Active Record 패턴**을 따른다.
Active Record는 **객체가 데이터베이스 테이블과 직접 연결되어 자신의 데이터를 스스로 저장·조회하는 방식**이다.

```python
class Article(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=50)

# 생성
article = Article(title="Django", author="Yeongrok")
article.save()

# 조회
found = Article.objects.get(id=1)
print(found.title)
```

이 구조에서 `Article` 클래스는 단순한 데이터 구조가 아니라,
데이터의 CRUD(Create, Read, Update, Delete)를 직접 수행하는 **도메인 객체**다.

즉, ORM이 SQL을 대신 처리하며 모델 클래스가 DB 작업을 모두 담당한다.
이는 **Django 모델이 엔티티(Entity)와 리포지토리(Repository)의 역할을 동시에 수행**한다는 뜻이다.

### Active Record의 장점

* 단순하고 직관적인 CRUD 코드 작성 가능
* 빠른 프로토타이핑과 MVP 개발에 적합
* ORM이 SQL을 추상화하므로 개발자가 비즈니스 로직에 집중 가능

### 한계

* 복잡한 비즈니스 로직이 모델에 집중되어 **Fat Model** 문제가 생길 수 있음
* 테스트나 유지보수 시, 데이터 접근 로직과 도메인 로직이 분리되지 않아 어려움이 발생

이 때문에 Django를 사용할 때는
Service Layer를 별도로 두거나, 로직을 분리하여 유지보수성을 높이는 접근이 권장된다.

---

## 4. Django의 도메인 중심 구조

Django 프로젝트 구조를 보면, 도메인 단위로 앱이 나뉘는 것을 알 수 있다.

```
project/
 ├── config/
 ├── apps/
 │    ├── users/
 │    ├── orders/
 │    ├── products/
 │    └── payments/
```

각 `app`은 특정 비즈니스 영역(도메인)을 표현하며,
모델, 뷰, URL, 테스트, 템플릿을 모두 포함한 **독립된 기능 단위**다.

이 구조 덕분에 Django는 기술적 계층보다 **업무 도메인 중심의 모듈화**가 자연스럽게 이루어진다.
즉, 하나의 `app`은 독립적인 하위 시스템으로 작동할 수 있다.

**장점**

* 도메인별 응집도(cohesion)가 높음
* 앱 간 결합도(coupling)가 낮아 유지보수에 유리함
* 특정 앱 단위로 테스트, 배포, 확장 가능

---

## 5. Layered Architecture와 비교

레이어드 아키텍처(Layered Architecture)는 기능을 기술적 역할에 따라 구분한다.

```
Controller → Service → Domain → Repository
```

각 레이어는 하위 레이어에만 의존하며,
**관심사의 분리(Separation of Concerns)**를 목표로 한다.
Flask, FastAPI, Spring 등의 프레임워크는 이 방식을 주로 따른다.

반면 Django는 이를 도메인 단위(`app`)로 묶는 구조를 갖는다.
즉, Django에서는 **각 도메인 내부에서 레이어드 구조가 암묵적으로 존재**하며,
도메인 자체가 프로젝트의 상위 구분 기준이 된다.

| 구분    | Layered Architecture             | Django Domain Structure          |
| ----- | -------------------------------- | -------------------------------- |
| 분리 기준 | 기술적 역할(API, Service, Repository) | 비즈니스 개념(Users, Orders, Products) |
| 의존 방향 | 수직적 (위 → 아래)                     | 수평적 (도메인 단위 내 응집)                |
| 중심 사고 | 기능 중심                            | 도메인 중심                           |

---

## 6. Clean Architecture와의 연결

현대적인 백엔드 구조는 레이어드와 도메인 구조를 통합한 형태로 발전하고 있다.
대표적인 형태가 **Clean Architecture** 또는 **Hexagonal Architecture**이다.

```
/src
 ├── users/
 │    ├── domain/ (Entity, Logic)
 │    ├── application/ (UseCase, Service)
 │    ├── infrastructure/ (DB, Cache)
 │    └── interface/ (API, Router)
 ├── products/
 │    └── ...
```

Django는 이미 이 구조의 기본 형태를 가지고 있다.
`app` 단위가 도메인 경계를 나타내며,
각 앱 내부에서 역할을 분리하면 Clean Architecture로 자연스럽게 확장할 수 있다.

---

## 7. 정리

| 항목          | Django 특징                               |
| ----------- | --------------------------------------- |
| **핵심 아키텍처** | MTV (Model–Template–View)               |
| **ORM 패턴**  | Active Record                           |
| **구조 단위**   | 도메인 중심 app 구조                           |
| **장점**      | 높은 생산성, 빠른 프로토타이핑, 일관된 구조               |
| **한계**      | 복잡한 도메인 로직 분리 어려움, Fat Model 문제         |
| **발전 방향**   | Service Layer 분리, Clean Architecture 도입 |

---

Django는 Active Record 기반의 MTV 구조를 사용하지만,
앱 단위로 도메인을 분리함으로써 도메인 중심 아키텍처에 가까운 형태를 제공한다.
이를 바탕으로 Service Layer나 Clean Architecture를 적용하면
대규모 시스템에서도 유연하고 유지보수성 높은 백엔드 구조를 구축할 수 있다.




