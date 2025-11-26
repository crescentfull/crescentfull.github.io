---
title: "self와 cls의 이해"
description: "python 객체의 구조 이해(self, cls)"
date: 2025-06-05 10:00:00 +0900
categories: [Language, Python]
tags: [Language, Python, Instance, OOP]
---


# self와 cls의 이해 — 인스턴스와 클래스 사이에서 길 찾기

> "Explicit is better than implicit." — *The Zen of Python*
>
> 파이썬은 **무엇이** 전달되는지 숨기기보다는 **명시**하도록 설계돼 있다. 그래서 우리는 메서드의 첫 번째 매개변수로 `self` 나 `cls` 를 *직접* 써야 한다.

---

## 목차

1. 무엇이 왜 ‘Explicit’ 한가 – 배경 이야기
2. 인스턴스 레벨의 주인공 **`self`**
3. 클래스 레벨의 지휘관 **`cls`**
4. `self` · `cls` · `@staticmethod` 한눈 비교
5. 흔한 실수 & 디버깅 치트시트
6. 한 걸음 더: 바운드 메서드 → descriptor → metaclass
7. 요약 📜 + 치트시트 이미지

---

## 1. 무엇이 왜 ‘Explicit’ 한가 – 배경 이야기

* C++·Java 는 메서드 호출 시 숨겨진 `this` 포인터를 자동으로 넘긴다.
* **Python** 은 "누가 호출했는지" 까지 시그니처에 적도록 한다. (컨벤션: `self`, `cls`)
* 덕분에 **동적 디스패치** 과정이 Python 레벨에서 보이므로, 메타프로그래밍 기법이나 디버깅 시 큰 도움이 된다.

```python
class Example:
    def method(self):      # self는 인스턴스!
        ...

    @classmethod
    def cmethod(cls):     # cls는 클래스!
        ...
```

> **📝 NOTE**  Python 3 부터는 "unbound method" 개념이 사라졌습니다. `Example.method` 는 그냥 **함수 객체**이며, 직접 호출하려면 인스턴스를 *수동*으로 넘겨야 합니다.

---

## 2. 인스턴스 레벨의 주인공 `self`

### 2‑1. 내가 곧 ‘그 객체’

```python
user = Account(balance=100)
user.deposit(50)   # self == user
```

* `id(self) == id(user)` — 두 객체는 동일 참조
* 메서드 안에서 `self.balance` 등을 읽고 쓸 수 있음

### 2‑2. 세탁기 예시 (상태가 서로 다른 인스턴스)

```python
class Washer:
    def __init__(self, mode="eco"):
        self.mode = mode
    def start(self):
        print(f"{self.mode} 모드로 세탁 시작!")

A = Washer("speed");  B = Washer("silent")
A.start()  # speed 모드
B.start()  # silent 모드
```

---

## 3. 클래스 레벨의 지휘관 `cls`

### 3‑1. 모든 인스턴스의 통제 센터

```python
class Employee:
    _seq = 0      # 클래스 변수, 모든 객체가 공유
    def __init__(self, name):
        self.name = name
        self.id = Employee._next_id()

    @classmethod
    def _next_id(cls):
        cls._seq += 1
        return cls._seq
```

> **⚠️ 멀티스레드 주의:** 동시에 여러 스레드가 객체를 생성한다면 `_seq` 증가를 `threading.Lock` 으로 감싸야 안전합니다.

### 3‑2. 팩토리 메서드

```python
@classmethod
def zero(cls):
    """새 객체를 기본값으로 만든다."""
    return cls(x=0)
```

---

## 4. `self` · `cls` · `@staticmethod` 한눈 비교

| 종류       | 첫 인자   | 언제 사용?           | 호출 예시            |
| -------- | ------ | ---------------- | ---------------- |
| 인스턴스 메서드 | `self` | 인스턴스 상태 접근·변경    | `obj.method()`   |
| 클래스 메서드  | `cls`  | 공통 상태·팩토리·대체 생성자 | `Class.method()` |
| 정적 메서드   | *(없음)* | 유틸 함수 (상태 독립)    | `Class.method()` |

```python
class ToolBox:
    @staticmethod
    def to_iso(dt):   # 날짜를 ISO‑8601 문자열로 변환
        return dt.astimezone(timezone.utc).isoformat()
```

---

## 5. 흔한 실수 & 디버깅 치트시트

| 실수 코드                       | 증상·Exception                                       | 올바른 호출                            |
| --------------------------- | -------------------------------------------------- | --------------------------------- |
| `def func2(): ...`          | `TypeError: func2() takes 0 positional…`           | `def func2(self):`                |
| `Class.func2()`             | `TypeError: func2() missing 1 required positional` | `obj.func2()`                     |
| `self.name = …` (클래스 변수 덮음) | 클래스 변수 그림자 생성 → 예상과 다른 동작                          | `type(self).name` 으로 접근하거나 변수명 구분 |
| 오타 `test.obj.func2()`       | `AttributeError: 'SelfTest' object has no attr…`   | `test_obj.func2()`                |

> **디버깅 꿀팁**  `print(id(self))`, `print(id(cls))` 로 주소 확인하면 헷갈림이 종결됩니다.

---

## 6. 한 걸음 더: 바운드 메서드 → descriptor → metaclass

### 6‑1. 바운드 메서드란?

```python
bound = user.deposit
print(bound.__self__ is user)  # True, self가 고정된 함수
```

### 6‑2. descriptor 매직 (ASCII 다이어그램)

```
Account.deposit (function object)
   │           ┌───────────┐  obj is None → 함수 그대로 반환
   └─.__get__(obj, type)──▶│  __get__    │──────────▶ <function deposit>
               └───────────┘
                   │  obj is user → 바운드 메서드
                   ▼
             <bound method user.deposit>
```

### 6‑3. 메타클래스

* **클래스도 객체** → `type` 의 인스턴스.
* ORM(예: Django), Enum 등이 metaclass 로 새로운 문법을 구현.

```python
class UpperAttrMeta(type):
    def __new__(mcls, name, bases, attrs):
        up_attrs = {k.upper(): v for k, v in attrs.items() if not k.startswith("__")}
        return super().__new__(mcls, name, bases, up_attrs)
```

---

## 7. 요약 📜

* **`self` ≡ 인스턴스 자신** (주소 동일)
* **`cls` ≡ 클래스 객체**  (모든 인스턴스 공유)
* **`@staticmethod`**  상태 독립 유틸 함수
* “정말 헷갈리면 **주소(id) 찍어** 보라!”

### 치트시트 그림 (alt‑텍스트)

```
┌─────────┐        ┌────────┐                  ┌──────────┐
│  self   │  ==>  │ 인스턴스 │   상태 보관        │  나 개인!  │
└─────────┘        └────────┘                  └──────────┘
┌─────────┐        ┌────────┐                  ┌──────────┐
│  cls    │  ==>  │ 클래스  │   공통 규칙        │ 우리 회사! │
└─────────┘        └────────┘                  └──────────┘
```

> **한 줄 결론**
> `self` 와 `cls` 를 명확히 구분하면, 클래스는 더 이상 *숙제* 가 아니라 **레고 블록** 이 된다. 🧩

“Python OOP 에서 self 와 cls 를 구분할 줄 알면,
클래스는 더 이상 거대한 ‘숙제’ 가 아니라,
유연한 레고 블록 으로 변신한다.”

코드를 읽다 헷갈리면 언제든 print(id(...)) 로 확인해 보자.
직접 눈으로 주소를 확인하는 순간, 두 개념이 머릿속에 착 달라붙을 것이다!