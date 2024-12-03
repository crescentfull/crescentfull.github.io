---
title: "데코레이터"
description: "함수를 감싸는 함수로, 기존 함수에 추가적인 기능을 부여하는 기법"
date: 2021-08-04 10:00:00 +0900
categories: [Language, Python]
tags: [Language, Python, Decorator]
---

# 데코레이터의 이해
## 중첩함수(nested function)
- 함수 내부에 정의된 또 다른 함수
- 중첩함수는 해당 함수가 정의된 함수 내에서 호출 및 반환 가능
- **함수 안에 선언된 변수는 함수 안에서만 사용 가능한 원리와 동일 (로컬 변수)**


파이썬 closure함수와 데코레이터 코드는 아주 비슷하다. 다만 함수를 다른 함수의 인자로 전달한다는 점이 조금 틀리다.

```python
# closure function
def def outer_function(msg):
    def inner_function():
        print(msg)

    return inner_function

hi_func = outer_function('Hi')
bye_func = outer_function('Bye')

hi_func()  # >>> hi
bye_func() # >>> bye
```
```python
# decorator
def decorator_function(original_function):  # 1
    def wrapper_function():  # 5
        return original_function()  # 7

    return wrapper_function  # 6


def display():  # 2
    print('display 함수가 실행 되었습니다.')  # 8


decorated_display = decorator_function(display)  # 3

decorated_display()  # 4 # >>> display 함수가 실행 되었습니다.
```
위의 코드는 다음과 같은 내용이다.

1. 데코레이터 함수인 decorator_function과 일반 함수인 display를 #1과 #2에서 각각 정의
2. 그 다음에 #3에서 decorated_display라는 변수에 display 함수를 인자로 갖은 decorator_function을 실행한 리턴값을 할당. (물론 이 리턴값은 wrapper_function이 되겠죠. 여기서 wrapper_function 함수는 아직 실행된게 아니다. decorated_display 변수 안에서 호출되기를 기다리는 것)
3. 그리고 #4의 decorated_display()를 통해 wrapper_function을 호출하면 #5번에서 정의된  wrapper_function이 호출 된다.
4. 그러면 #7에서 original_function인 display 함수가 호출되어 #8의 print 함수가 호출되고 문자열이 출력!

## 데코레이터를 왜 쓰는 걸까

데코레이터라는 것을 도데체 왜 쓰는걸까? 그 이유는 이미 만들어져 있는 기존의 코드를 수정하지 않고도, 래퍼(wrapper) 함수를 이용하여 여러가지 기능을 추가할 수가 있기 때문이다. 

---
Reference
- [스쿨오브웹-파이썬_데코레이터](https://schoolofweb.net/blog/posts/%ed%8c%8c%ec%9d%b4%ec%8d%ac-%eb%8d%b0%ec%bd%94%eb%a0%88%ec%9d%b4%ed%84%b0-decorator/)
