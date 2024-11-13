---
title: "Set with Python"
description: "Set with Python"
date: 2022-02-03 11:00:00 +0900
categories: [CS, Data Structure]
tags: [Computer Science, Data Structure, Set, Python]
# image: 
---
# what is the SET ?!

## set

### 특징

- 집합에 관련된 것을 쉽게 처리하기 위해 만든 자료형이다.
- 중복을 허용하지 않는다.
- 순서가 없어 인덱스로 값을 얻을 수 없다.
-----

### 1. 생성 
```python
a = {1, 2, 3, 4, 5}
print(type(a)) #<class 'set'>

b = {}
print(type(b)) #<class 'dict'>

c = set()
print(type(c)) #<class 'set'>
```

<br>

### 2. 추가&삭제
```python
#추가
a = {1, 2, 3}
a.add(4)
print(a)

>>> {1, 2, 3, 4}


#여러 값 추가
a = {1, 2, 3}
a.update({4, 5, 6})
print(a)

>>> {1, 2, 3, 4, 5, 6}


#삭제
a = {1, 2, 3}
a.remove(2)
print(a)

>>> {1, 3}
```

<br>

### 3. 교집합&합집합&차집합
- 메서드나 연산자를 사용하여 구할 수 있다.

```python
set1 = {1, 2, 3, 4, 5, 6}
set2 = {4, 5, 6, 7, 8, 9}

#교집합
print(set1 & set2)
>>> {4, 5, 6}
print(set1.intersection(set2))
>>> {4, 5, 6}

#합집합
print(set1 | set2)
>>> {1, 2, 3, 4, 5, 6, 7, 8, 9}
print(set1.union(set2))
>>> {1, 2, 3, 4, 5, 6, 7, 8, 9}

#차집합
print(set1 - set2)
>>> {1, 2, 3}
print(set1.difference(set2))
>>> {1, 2, 3}
```

<br>

### 4. list VS set (in 연산 시간복잡도 비교)

- List 자료형에서는 in 연산자의 시간 복잡도는 O(n)이고,
- set 자료형에서는 in 연산자의 시간 복잡도는 O(1)이다.

```python
import timeit

s = set(range(0, 1000000))
l = list(range(0, 1000000))

# set
start = timeit.default_timer()
print(999999 in s)
print(timeit.default_timer() - start) 

# list
start = timeit.default_timer()
print(999999 in l)
print(timeit.default_timer() - start)  

print('-----------------------------------')

#set
start = timeit.default_timer()
for i in range(1000):
    i+1000 in s
print(timeit.default_timer() - start) 

#list 
start = timeit.default_timer()
for i in range(1000):
    i+1000 in l
print(timeit.default_timer() - start) 


>>>
True
0.00001733399403747171
True
0.010281834998750128
-----------------------------------
0.00009790800686459988
0.015224540999042802
```