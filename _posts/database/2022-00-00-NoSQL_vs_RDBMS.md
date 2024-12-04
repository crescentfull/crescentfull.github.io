---
title: "RDBMS vs NoSQL"
description: "RDBMS와 NoSQL의 특징, 장단점, 언제 사용되는지"
date: 2022-8-20 10:00:00 +0900
categories: [CS, Database]
tags: [Computer Science, Database, RDBMS, NoSQL]
---

# Database? SQL? DBMS?

- **Database** : 컴퓨터 시스템에 전자 방식으로 저장된 구조화된 정보 혹은 <u>데이터의 체계적인 집합체</u>를 의미

- **DBMS(DataBase Management System)** : 사용자와 데이터베이스 사이에서 사용자의 요구에 따라 정보를 생성해 주고 데이터베이스를 관리해 주는 '**소프트웨어**'
- **SQL(Strucured Query Language)** : <u>관계형 데이터베이스</u> 관리 시스템의 데이터를 관리하기 위해 설계된 '**특수 목적의 프로그래밍 언어**'이며, 관계형 데이터베이스 관리 시스템에서 자료의 검색과 관리, 데이터베이스 스키마 생성과 수정, 데이터베이스 객체 접근 조정 관리를 위해 고안되었음
- **스키마(Schema)** : 데이터베이스를 구성하는 **개체(Entity),속성(Attribute),관계(Relationship)** 및 제약조건 등에 관해 전반적으로 정의한 메타데이터의 집합

## R(relational)DBMS
- 저장방식 : sql 문법에 의해 저장되며 정해진 스키마에 따라서 데이터 저장
- RDB를 관리하는 시스템, RDB는 관계형 데이터 모델을 기초로 모든 데이터를 2차원 테이블 형태로 표현하는 데이터베이스 = **관계형 데이터베이스 관리 시스템**
- **Attribute와 Value**를 이용하여 데이터를 정의하고 저장·관리
>![](https://images.velog.io/images/sicksong/post/6aedc798-e51e-4cf5-b401-fe7ec01abba9/image.png) Attribute => 고객/나이/성별
value => 김가가,박나나 / 11,13 / 남,여

- 각 테이블들이 관계를 나타내기 위해서 **외래 키(foreign key)**를 사용
> 부모(primary key)에 자식키(foreign key)가 참조
- 이러한 테이블간의 관계에서 외래키를 이용하여 **join이 가능**하다는게 큰 특징

## NoSQL(Not Only SQL)
- 관계형 데이터베이스와는 반대되는 방식
- <u>테이블끼리의 관계를 정의하지 않는다.</u> 그러므로 **join도 불가능**
   - 정해진 스키마가 없어서 보다 자유롭게 데이터를 저장할 수 있다.
- 빅데이터의 등장으로 데이터와 트래픽이 기하급수적으로 증가함에 따라 생겼다. 왜?
   - RDBMS에 단점인 성능을 향상시키기 위해서는 장비가 좋아야한다. 
   - 즉, RDBMS의 Scale-Up 특징이 비용을 엄청나게 증가시키기 때문에 데이터의 일관성을 포기하고 비용을 아끼기 위해서 여러 대의 데이터에 분산하여 저장하는 Scale-Out을 쓰기위해 만들어진 것이다.
- key 값만 가지고 데이터에 대한 입·출력이 가능
> 고객 id(key값) : aaa / 고객정보 : 가가가,11,남
> 고객 id(key값) : bbb / 고객정보 : 나나나,13,여

## RDBMS & NoSQL 장단점
### RDBMS
- #### 장점
  - 정해진 스키마에 따라 데이터를 저장하여야 하므로 명확한 데이터 구조를 보장(Data를 Column과 Row형태로 저장)
  - 관계는 각 데이터를 중복없이 한 번만 저장할 수 있다. => 정규화를 하자!
  - 데이터의 분류, 정렬, 탐색 속도가 비교적 빠름
  - 데이터의 Update가 빠름
- #### 단점
  - 데이터 처리의 부하시 처리가 어렵다
  - 반드시 스키마 규격에 맞춰서 데이터 처리

### NoSQL
- #### 장점
  - 데이터 간의 관계를 정의하지 않아서 자유롭다.=> 데이블간의 관계(join)이 불가능·불필요
  - RDBMS보다 복잡함이 떨어지기 때문에 훨씬 대용량의 데이터를 저장·관리
  - 테이블에 스키마가 정해져 있지 않아서 데이터 저장이 비교적 자유롭다
  - 언제든 저장된 데이터를 조정하고 새로운 필드를 추가할 수 있다.
  - 데이터 분산이 용이하여 Scale-Up 뿐만 아니라 Scale-Out도 가능하다
- #### 단점
  - 데이터 중복이 발생할수 있다
  - 중복된 데이터가 변경될 경우 수정을 모든 컬렉션에서 진행해야 된다.
  - 스키마가 정해져 있지 않기 때문에 명확한 데이터 구조를 보장하지 않으며 데이터 구조 결정하기가 어려울 수 있다.


## (총정리) RDBMS, NoSQL 언제 사용 되나?
- RDBMS는 데이터 구조가 명확하고, 변경될 여지가 없으며, 명확한 스키마가 중요한 경우. 또한 중복된 데이터가 없어서(데이터 무결성) 변경이 용이하기 때문에 관계를 맺고 있는 데이터가 자주 변경이 이루어지는 시스템에 적합
- NoSQL은 정확한 데이터 구조를 알 수 없고 데이터가 변경/확장이 될 수 있는 경우에 사용하는 것이 좋다. 하지만 데이터 중복이 발생할 수 있으며 중복된 데이터가 발생 시 모든 컬렉션에서 수정을 해야하기 때문에 update가 자주 이루어지지 않는 시스템에 알맞다. 또한 Scale-Out이 가능하다는 장점을 활용해 막대한 데이터를 저장해야되는 시스템에 적합하다.

![](https://images.velog.io/images/sicksong/post/6c38c175-7d87-4094-a7a2-9368dc3aebd0/image.png)
   
![](https://images.velog.io/images/sicksong/post/8c554a35-3f2b-4658-855a-15bcc9a68b01/image.png)

******
참조 블로그
- https://pythontoomuchinformation.tistory.com/528
- https://bbinya.tistory.com/12
- https://universitytomorrow.com/26
- https://m.blog.naver.com/islove8587/220548900044