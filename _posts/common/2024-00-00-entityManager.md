---
title: "엔티티매니저 EntityManager"
description: "EntityManager는 JPA에서 엔티티와 영속성 컨텍스트를 다루는 핵심 개체"
date: 2024-12-13 10:00:00 +0900
categories: [CS, Common]
tags: [JPA, EntityManager]
---

# 엔티티매니저 EntityManager
- EntityManager는 JPA에서 엔티티와 영속성 컨텍스트를 다루는 핵심 개체이다.
- 영속성 컨텍스트를 통해 엔티티의 상태를 관리하며, 트랜잭션 내부에서 안전하게 DB와 상호작용 하도록 돕는다.

그렇다면 영속성이란 무엇일까

## 영속성 컨텍스트에 대해 이해
  - 엔티티를 영구 저장하는 환경으로 1차 캐시, 쓰기 지연, 변경 감지 등 다양한 기능을 제공함  
  - 동일한 트랜잭션 범위 내에서 동일한 엔티티를 반복 조회할 때 DB가 아닌 1차 캐시에서 조회함  
  - 엔티티가 변경될 때, 쓰기 지연 저장소에 쿼리를 모아두었다가 트랜잭션 종료 시점에 flush 과정을 통해 DB와 동기화함  

## 엔티티 매니저(EntityManager)의 역할 
  - 영속성 컨텍스트에 엔티티를 등록(persist)하거나, 삭제(remove), 다시 영속화(merge), 준영속(detach) 상태로 전환시킴  
  - JPQL이나 Native Query를 사용해 직접 DB로부터 데이터를 조회하거나, 1차 캐시를 통해 이미 조회한 엔티티를 재활용함  
  - 트랜잭션 범위 내에서 엔티티 상태 전환과 DB 동기화를 제어하고, 필요 시 flush를 사용해 원하는 시점에 DB 반영 가능함  
  - 영속성 컨텍스트가 close되거나 clear되면, 해당 범위에 있던 영속 상태의 엔티티는 준영속 상태로 전환됨  

### 엔티티의 4가지 상태
  1. **비영속**  
     - 새로 생성된 엔티티가 아직 영속성 컨텍스트와 연결되지 않은 상태  
     - 예시 코드:  
       ```java
       // Member 엔티티를 새로 생성했으나, 아직 비영속 상태
       Member member = new Member("산초");
       ```
  2. **영속**  
     - 영속성 컨텍스트에 연결된 상태로, 변경 사항이 자동으로 DB에 반영되는 상태  
     - 예시 코드:  
       ```java
       // 비영속 상태의 member를 영속성 컨텍스트에 등록
       em.persist(member);
       ```
  3. **준영속**  
     - 한 번 영속 상태였다가 영속성 컨텍스트와 분리된 상태로, 변경 사항이 더 이상 DB에 반영되지 않는 상태  
     - 예시 코드:  
       ```java
       // 영속성 컨텍스트와 분리해 준영속 상태로 만든다
       em.detach(member);
       em.clear(); 
       em.close(); 
       ```
  4. **삭제**  
     - 영속 상태의 엔티티를 영속성 컨텍스트와 DB에서 완전히 제거한 상태  
     - 예시 코드:  
       ```java
       // 영속 상태의 member를 삭제 상태로 전환
       em.remove(member);
       ```  

- **간단한 예시 코드로 살펴보기**  
  ```java
  public class Main {
      public static void main(String[] args) {
          EntityManagerFactory emf = Persistence.createEntityManagerFactory("example-unit");
          EntityManager em = emf.createEntityManager();
          EntityTransaction tx = em.getTransaction();
          
          tx.begin();
          try {
              // 1. 비영속 상태
              Member member = new Member("산초");
              
              // 2. 영속 상태로 전환 (persist)
              em.persist(member);
              
              // 3. 준영속 상태로 전환 (detach)
              em.detach(member);
              
              // 4. 다시 영속 상태로 전환 (merge)
              Member mergedMember = em.merge(member);
              
              // 엔티티 매니저에서 삭제 상태로 전환
              em.remove(mergedMember);
              
              tx.commit();
          } catch(Exception e) {
              tx.rollback();
          } finally {
              em.close();
              emf.close();
          }
      }
  }
  ```
  - `em.persist(member)` : 비영속 상태의 `member` 엔티티를 영속 상태로 전환함  
  - `em.detach(member)` : `member` 엔티티를 영속성 컨텍스트에서 분리해 준영속 상태로 만듦  
  - `em.merge(member)` : 준영속 상태의 엔티티를 다시 영속 상태로 복귀시킴  
  - `em.remove(mergedMember)` : 영속 상태의 엔티티를 삭제 상태로 전환함  

## 그래서 엔티티 매니저란 것은 결국?  
  - 엔티티 매니저는 영속성 컨텍스트를 통해 엔티티의 생명 주기를 일관성 있게 관리함  
  - 비영속, 영속, 준영속, 삭제 상태로 나뉜 엔티티를 `persist`, `merge`, `remove`, `detach` 같은 메서드로 조작함  
  - 1차 캐시와 쓰기 지연, 변경 감지 기능을 사용해 DB와의 직접적인 통신 횟수를 줄이고 성능을 높임  
  - 결과적으로 객체지향적인 접근 방식을 유지하면서도 안전하고 유연하게 데이터베이스와 연동하게 됨