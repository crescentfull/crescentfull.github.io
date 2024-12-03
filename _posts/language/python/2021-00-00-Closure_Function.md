---
title: "클로저 함수"
description: "함수오 해당 함수가 가지고 있는 데이터를 함께 복사, 저장해서 별도의 함수로 활용하는 기법"
date: 2021-07-31 10:00:00 +0900
categories: [Language, Python]
tags: [Language, Python, Closure Function]
---

# closure function 
- 함수와 해당 함수가 가지고 있는 데이터를 함께 복사, 저장해서 별도의 함수로 활용하는 기법
- 외부 함수가 소멸되더라도, 외부 함수 안에 있는 로컬 변수 값과 중첩함수(내부함수)를 사용할 수 있는 기법
- 함수형 프로그래밍에서 고안되었음

```python
def outer_func(num):
    # 중첩 함수에서 외부 함수의 변수에 접근 가능
    def inner_func():
        print(num)
        return '안녕'
    
    return inner_func                 # 중첩 함수 이름을 리턴합니다.
    
closure_func = outer_func(10)    # <--- First-class function
closure_func()            # <--- Closure 호출 
```
> 위의 예제에서 closure_func이 바로 closure 임
closure_func = outer_func(10) 에서 outer_func 함수는 호출 종료
closure_func() 은 결국 inner_func 함수를 호출
outer_func(10) 호출 종료시 num 값은 없어졌으나, closure_func()에서 inner_func이 호출되면서 이전의 num값(10)을 사용함

## 언제 closure를 사용할까?
- closure는 객체와 유사
- 일반적으로 제공해야할 기능(method)이 적은 경우, closure를 사용하기도 함
- 제공해야할 기능(method)가 많은 경우등은 class를 사용하여 구현

---
‼️ 조금 더 자세한 내용은
### [스쿨오브웹-클로저](https://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%81%B4%EB%A1%9C%EC%A0%80-closure/)