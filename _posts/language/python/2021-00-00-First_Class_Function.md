---
title: "First Class Function"
description: "일급 함수, 특정 프로그래밍 언어에서 함수가 변수처럼 다뤄지는 경우 일급 함수라 칭함"
date: 2021-07-30 10:00:00 +0900
categories: [Language, Python]
tags: [Language, Python, First Class Function]
---

# First Class function
프로그래밍 언어가 함수 (function) 를 first-class citizen으로 취급하는 것을 뜻한다. 
쉽게 설명하자면 '함수 자체'를 '인자 (argument) 로써' 다른 함수에 전달하거나 다른 함수의 결과값으로 리턴 할수도 있고, 함수를 변수에 할당하거나 데이터 구조안에 저장할 수 있는 함수를 뜻한다.
> first-class citizen(1급 객체)
아래 3가지 조건을 충족한다면 1급 함수라 할 수 있다.
1. 변수나 데이터에 할당 할 수 있어야 한다.
2. 객체의 인자로 넘길 수 있어야 한다.
3. 객체 리턴값으로 리턴 할수 있어야 한다.

사실상 python에서 모든 함수는 first-class 이다.
- 런타임에 생성할 수 있다
- 데이터 구조체의 변수나 요소에 할당할 수 있다
- 함수 인수로 전달할 수 있다
- 함수 결과로 리턴할 수 있다
  - 위와 같은 작업을 수행할 수 있으면 프로그램 개체를 first-class로 정의한다
  
## 이미 정의된 여러 함수를 간단히 재사용 할 수 있다.
```python
def squre(x):
  return x*x

def cube(x):
  return x*x*x

def quad(x):
  return x*x*x*x
  
def power(func, arr):
  result = []
  for i in arr:
    result.append(func(i))
  return result

num = [1,2,3,4,5]

print(power(square, num))
>> [1, 4, 9, 16, 25]

print(power(cube, num))
>> [1, 8, 27, 64, 125]

print(power(quad, num))
>> [1, 16, 81, 256, 625]
```
---
### Reference
- [스쿨오브웹-파이썬 퍼스트 클래스](https://schoolofweb.net/blog/posts/%ed%8c%8c%ec%9d%b4%ec%8d%ac-%ed%8d%bc%ec%8a%a4%ed%8a%b8%ed%81%b4%eb%9e%98%ec%8a%a4-%ed%95%a8%ec%88%98-first-class-function/)
---