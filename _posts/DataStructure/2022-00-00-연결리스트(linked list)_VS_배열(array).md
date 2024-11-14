---
title: "연결리스트(linked list) VS 배열(array)"
description: "연결리스트(linked list) VS 배열(array)"
date: 2022-02-02 10:00:00 +0900
categories: [CS, Data Structure]
tags: [Computer Science, Data Structure, Linked List, Array]
# image: 
---

# 차이점

## 1. Search(검색)
>### Array
- Arrary는 **Random Access를 지원**하므로 element들을 **index를 통해 직접적으로 접근 가능**
- 특정 element 접근하는 ```시간 복잡도 O(1)```

> ### Linked List
- Linked List는 **Sequential Access를 지원**하므로 element/node에 접근할 때 **처음부터 순차적으로 접근**하여 찾아야함
- 특정 element 접근하는 ```시간 복잡도 O(n)```

## 2. Insert(삽입)
>### Array
- Array는 데이터들이 순차적으로 저장되어있으므로 맨 처음이나 그 이후에 데이터가 추가될 경우 그 뒤에 있는 데이터들을 모두 한 칸씩 뒤로 미뤄야함
- 추가하려는 데이터가 맨뒤가 아니라면 ```O(n)의 시간복잡도```
- 추가하려는 데이터의 위치가 맨 뒤이고 배열에 공간이 남는다면 ```O(1)의 시간복잡도```

>### Linked List
- 데이터를 추가하는 행위 자체의 ```시간복잡도는 O(1)```
- **데이터의 위치가 맨 처음이 아닌 그 이후라면 순차적으로 탐색하여 해당위치  까지가 가야함**
- 추가하려는 데이터의 위치가 맨 앞이라면 ```O(1)의 시간복잡도```
- 추가하려는 데이터의 위치가 맨 앞 그 이후라면 ```O(n)의 시간복잡도```

## 3. Deletion(삭제)
>### Array
- Array는 데이터들이 순차적으로 저장되어있으므로 **맨 처음이나 그 이후에 데이터가 삭제될 경우 그 뒤에 있는 데이터들을 모두 한 칸씩 앞으로 당겨야함**
- 삭제하려는 데이터가 맨뒤가 아니라면 ```O(n)의 시간복잡도```
- 삭제하려는 데이터의 위치가 맨 뒤이고 배열에 공간이 남는다면 ```O(1)의 시간복잡도```

>### Linked List
- 데이터를 삭제하는 행위 자체의 ```시간복잡도는 O(1)```
- **데이터의 위치가 맨 처음이 아닌 그 이후라면 순차적으로 탐색하여 해당위치 까지가 가야함**
삭제하려는 데이터의 위치가 맨 앞이라면 ```O(1)의 시간복잡도```
삭제하려는 데이터의 위치가 맨 앞 그 이후라면 ```O(n)의 시간복잡도```

## 4. 저장 방식
>### Array
- Array에서 element들은 인접한 memory 위치에 저장되거나 memory에 연이어 저장

>### Linked List
- Linked List에서 새로운 element/node들은 memory 어딘가에 저장
- 새로운 element/node에 할당된 memory 위치 주소는 linked list의 이전 node에 저장

## 5. Memory Allocation(메모리 할당)
>### Array
- Stack section에 메모리 할당이 이루어짐
- Memory는 Array가 선언되자마자 **Compile time에 할당**
- ```Static Memory Allocation```이라고 부름

>### Linked List
- Heap section에 메모리 할당이 이루어짐
- Memory는 새로운 node가 추가될 때 **runtime에 할당**
- ```Dynamic Memory Allocation```이라고 부름

## 6. Size(크기)
>### Array
- Array의 size는 array 선언 시점에 지정

>### Linked List
- Linked List의 size는 다양할 수 있음
✅ node들이 추가될 때 runtime 시점에서 size가 커질 수 있기 때문

---
## 결론
데이터 접근이 중요하다면 Array
데이터 수정(삽입 및 삭제)이 중요하다면 Linked List

*출처
https://velog.io/@alsrn7590/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-ArrayLinked-List