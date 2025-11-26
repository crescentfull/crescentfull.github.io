---
title: "테스트 자동화, 왜 시작해야 할까?"
description: "테스트 자동화의 개념과 필요성, 그리고 다양한 테스트 유형에 대해 알아보자"
date: 2025-07-01 10:00:00 +0900
categories: [Common, Testing]
tags: [test, automation, pytest, selenium, ci-cd]
---

# Chapter 1. 테스트 자동화, 왜 시작해야 할까?

## 1.1. 테스트란 무엇인가

테스트는 **소프트웨어가 의도한 대로 동작하는지 검증하는 행위**이다.
프로그램이 아무리 잘 작성되었더라도, 기대와 다르게 작동할 가능성은 늘 존재한다.
이러한 가능성을 줄이고, 사용자에게 신뢰할 수 있는 기능을 제공하기 위해 우리는 테스트를 수행한다.

테스트는 **"신뢰"를 보장하기 위한 투자**이며, 이는 특히 소프트웨어가 복잡해질수록 더욱 중요해진다.

---

## 1.2. 수동 테스트의 한계

많은 개발 초기 프로젝트에서 테스트는 사람의 손으로 수행된다.
기능을 구현하고, 브라우저를 열고, 버튼을 클릭해본다. 데이터를 입력하고 결과를 확인한다.

이 방식은 빠르게 시작할 수 있다는 장점이 있지만, 다음과 같은 문제점을 갖고 있다.

* 사람이 직접 수행해야 하므로 **반복성이 떨어진다**
* 테스트를 놓치는 경우가 많다 (예: "이번엔 대충 봐도 되겠지"라는 생각)
* 시간과 체력의 한계로 인해 **빈번한 테스트가 어렵다**
* 협업 환경에서는 테스트의 기준이 명확하지 않아 **불일치가 발생할 수 있다**

이러한 한계를 해결하기 위한 방법이 바로 **테스트 자동화**이다.

---

## 1.3. 생성형 AI의 부상과 QA의 역할 변화

생성형 AI의 도입으로 개발 속도는 비약적으로 향상되었으며, 이에 따라 테스트의 중요성도 함께 커졌다. 맥킨지 보고서에 따르면 생성형 AI 도구를 사용하는 개발자는 두 배 빠르게 코드를 완성하지만, QA 부서의 테스트 대응 속도가 이를 따라가지 못한다면 품질 문제는 더 심각해질 수 있다.

이제 QA 엔지니어는 테스트 수행자가 아닌, 테스트 전략을 설계하고 AI를 활용하여 테스트 품질을 높이는 전략가로서의 역할을 수행해야 한다

---

# Chapter 2. 테스트 자동화란 무엇인가

## 2.1. 개념 정의

**테스트 자동화(Test Automation)** 란,
테스트 시나리오를 코드로 작성하고, 사람이 직접 개입하지 않아도 자동으로 테스트가 수행되도록 만드는 것이다.

예를 들어, "로그인 버튼을 누르면 홈 화면으로 이동해야 한다"는 동작을 코드로 작성하고,
이 테스트를 서버가 시작할 때마다 자동으로 검증할 수 있다.

자동화는 단순히 테스트를 빠르게 실행하는 것 이상의 의미를 갖는다.
이는 곧 **지속적인 품질 확보(CQ: Continuous Quality)** 를 가능하게 한다.

---

## 2.2. 테스트 자동화가 필요한 이유

자동화를 통해 우리는 다음과 같은 가치를 얻게 된다.

* **변화에 강한 코드**
  기능을 추가하거나 수정해도 기존 기능이 깨지지 않았음을 자동으로 확인할 수 있다.

* **빠른 피드백**
  코드를 작성한 직후 결과를 확인할 수 있어, 디버깅 시간과 비용을 절감할 수 있다.

* **CI/CD와 통합**
  배포 전에 모든 테스트가 자동으로 실행되어, 오류 있는 코드가 운영 환경에 반영되는 일을 방지할 수 있다.

---

# Chapter 3. 테스트의 분류

## 3.1. 단위 테스트 (Unit Test)

**단위 테스트**는 프로그램의 가장 작은 구성 요소, 즉 **하나의 함수나 메서드**가 올바르게 동작하는지를 검증한다.

* 예: `add(2, 3)` 함수가 `5`를 반환하는지 확인
* 장점: 빠르게 실행되며, 버그를 빠르게 감지할 수 있다

```python
def add(a, b):
    return a + b

def test_add():
    assert add(2, 3) == 5
```

## 3.2. 통합 테스트 (Integration Test)

여러 개의 모듈이 **함께 동작할 때 발생하는 상호작용**을 검증한다.
예를 들어, 데이터베이스와 웹 서버가 잘 연동되는지를 확인하는 테스트이다.

* 예: 유저 회원가입 → 데이터베이스에 저장 여부 확인

## 3.3. E2E 테스트 (End-to-End Test)

사용자 입장에서 **전체 시스템의 흐름을 시뮬레이션**한다.
실제 브라우저에서 버튼을 클릭하고 페이지가 전환되는 과정을 테스트한다.

* 예: 사용자가 로그인 버튼을 클릭하면 홈 화면으로 이동하는지 확인

## 3.4. 회귀 테스트 (Regression Test)

코드를 수정한 이후에도 **기존 기능이 여전히 잘 작동하는지 검증**하는 테스트이다.
회귀(regression)란, 이전에 잘 작동하던 기능이 변화 이후에 다시 오류를 일으키는 현상을 뜻한다.

---

# Chapter 4. 어떤 도구들이 있을까?

다양한 프로그래밍 언어와 테스트 목적에 따라 여러 도구들이 존재한다.

| 테스트 목적  | 대표 도구                                    | 사용 언어            |
| ------- | ---------------------------------------- | ---------------- |
| 단위 테스트  | `pytest`, `unittest`                     | Python           |
| API 테스트 | `Postman`, `REST-assured`                | JS, Java         |
| E2E 테스트 | `Selenium`, `Cypress`, `Playwright`      | JS, Python       |
| 성능 테스트  | `JMeter`, `k6`, `Locust`                 | Java, JS, Python |
| CI 연동   | `GitHub Actions`, `Jenkins`, `GitLab CI` | 언어 무관            |

---

# Chapter 5. 테스트 자동화는 언제 도입해야 할까?

테스트 자동화는 **초기부터** 도입하는 것이 가장 좋다.
하지만 현실적으로 다음과 같은 시점이 적기일 수 있다.

* **기능이 자주 변경되는 경우**
* **협업하는 팀원이 많아지는 경우**
* **배포 주기가 짧고 빠른 피드백이 필요한 경우**
* **서비스가 점점 커지고 수동 테스트가 어려워질 때**

---

테스트 자동화는 단순히 개발자의 편의를 위한 도구가 아니다.
이는 제품의 품질을 장기적으로 유지하고, 변화에 유연하게 대응할 수 있는 **기반 인프라**이다.

초기에는 학습 곡선이 있을 수 있으나,
한 번 습관화되면 더 나은 코드, 더 빠른 배포, 더 적은 버그라는 세 가지 혜택을 얻게 된다.

---

# Chapter 6. Pytest로 시작하는 테스트 자동화

## 6.1. 왜 pytest인가?

Python에는 기본 테스트 프레임워크로 `unittest`가 존재한다. 하지만 `pytest`는 다음과 같은 이유로 더 많이 사용된다.

* **더 간결한 문법**: 클래스를 만들지 않아도 테스트 가능
* **강력한 기능**: fixture, mark, parameterize 등 다양한 고급 기능 지원
* **뛰어난 확장성**: 다양한 플러그인과 CI 도구와의 통합이 쉬움
* **가독성 높은 에러 메시지**: 어떤 테스트가 왜 실패했는지 직관적으로 보여줌

---

## 6.2. Pytest 설치 및 구조

```bash
pip install pytest
```

테스트 파일은 다음과 같은 이름을 따라야 한다:

* 파일명: `test_*.py` 또는 `*_test.py`
* 함수명: `test_`로 시작

```plaintext
my_project/
├── app/
│   └── calculator.py
├── tests/
│   └── test_calculator.py
```

---

## 6.3. 기본 예제

**`app/calculator.py`**

```python
def add(a, b):
    return a + b
```

**`tests/test_calculator.py`**

```python
from app.calculator import add

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
```

실행 방법:

```bash
pytest
```

---

## 6.4. 실패한 테스트는 어떻게 보일까?

예를 들어, 다음 테스트가 실패한다고 가정해보자:

```python
def test_fail_example():
    assert 2 * 2 == 5
```

`pytest`는 다음처럼 **어떤 값이 달랐는지 상세하게 출력**해준다:

```
>       assert 2 * 2 == 5
E       assert 4 == 5
```

---

## 6.5. fixture로 반복 작업 줄이기

여러 테스트에서 같은 데이터를 반복해 쓴다면 `fixture`를 사용해 재사용할 수 있다.

```python
import pytest

@pytest.fixture
def sample_data():
    return {"a": 1, "b": 2}

def test_sum(sample_data):
    assert sample_data["a"] + sample_data["b"] == 3
```

---

## 6.6. 다양한 테스트 케이스: parametrize

하나의 테스트 함수로 여러 케이스를 테스트할 수 있다.

```python
import pytest

@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (0, 0, 0),
    (-1, -1, -2),
])
def test_add_param(a, b, expected):
    assert a + b == expected
```

---

## 6.7. Django와 함께 사용하기: pytest-django

`pytest`는 Django 프로젝트와도 잘 연동된다.

```bash
pip install pytest-django
```

`pytest.ini` 파일을 생성하고 Django 설정을 지정한다:

```ini
# pytest.ini
[pytest]
DJANGO_SETTINGS_MODULE = myproject.settings
```

Django ORM이 필요한 테스트의 경우, `@pytest.mark.django_db` 데코레이터를 붙여야 한다:

```python
import pytest
from myapp.models import User

@pytest.mark.django_db
def test_user_creation():
    user = User.objects.create(username="test")
    assert user.username == "test"
```

---

## 6.8. CI 환경에서 pytest 실행하기

`pytest`는 GitHub Actions, GitLab CI, Jenkins 등 다양한 CI 도구와 쉽게 연동된다.

예: GitHub Actions 설정 (`.github/workflows/test.yml`)

```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest
      - name: Run tests
        run: pytest
```

---

`pytest`는 단순한 테스트부터 복잡한 테스트 시나리오까지 폭넓게 대응할 수 있는 **Python 생태계의 대표적인 테스트 프레임워크**이다.
특히 Django 프로젝트와도 잘 어울리며, CI 환경과 연동해 **신뢰성 있는 개발 파이프라인**을 만드는 데 핵심적인 역할을 한다.

---

# Chapter 7. 실습으로 익히는 Pytest + GitHub Actions 테스트 자동화

---

## 7.1. 프로젝트 구조 설정

우선, 아래와 같은 단순한 Python 프로젝트를 만들어봅니다.

```plaintext
myapp/
├── app/
│   └── calculator.py
├── tests/
│   └── test_calculator.py
├── requirements.txt
├── pytest.ini
└── .github/
    └── workflows/
        └── test.yml
```

### 가상환경 설정

```bash
python -m venv venv
source venv/bin/activate  # Windows는 venv\Scripts\activate
```

---

## 7.2. 실제 기능 코드 작성

**`app/calculator.py`**

```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero.")
    return a / b
```

---

## 7.3. 단위 테스트 코드 작성

**`tests/test_calculator.py`**

```python
import pytest
from app.calculator import add, subtract, divide

def test_add():
    assert add(2, 3) == 5

def test_subtract():
    assert subtract(10, 4) == 6

def test_divide():
    assert divide(8, 2) == 4.0

def test_divide_by_zero():
    with pytest.raises(ValueError):
        divide(10, 0)
```

---

## 7.4. 테스트 실행해보기

```bash
pip install pytest
pytest
```

출력 예시:

```
=================== test session starts ===================
collected 4 items

tests/test_calculator.py ....                         [100%]

==================== 4 passed in 0.03s ====================
```

---

## 7.5. requirements.txt 생성

**`requirements.txt`**

```
pytest==7.4.0
```

(※ 프로젝트에 따라 다른 의존성이 있다면 함께 추가)

---

## 7.6. GitHub Actions 설정 (CI 자동화)

**`.github/workflows/test.yml`**

```yaml
name: Run Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Run tests
      run: pytest
```

---

## 7.7. 커밋하고 GitHub로 푸시

```bash
git init
git remote add origin https://github.com/your-username/myapp.git
git add .
git commit -m "init: add calculator with pytest"
git push -u origin main
```

### 🔁 결과 확인

1. GitHub → \[Actions] 탭 클릭
2. 자동으로 `Run Tests` 워크플로우 실행
3. 모든 테스트가 통과했는지 확인

---

## 7.8. 실패 시 행동 살펴보기

예를 들어 `add(2, 3)`이 `6`이라고 잘못 테스트하면, GitHub Actions는 `pytest` 실패를 감지하고 다음과 같이 로그를 남긴다:

```
>       assert add(2, 3) == 6
E       assert 5 == 6
```

CI/CD 환경에서도 테스트 실패 시 **머지 금지**, **슬랙 알림**, **배포 중단** 등을 연동할 수 있다.
