---
title: 스프링 JPA 영속성 컨텍스트
summary: 스프링 JPA 영속성 컨텍스트란?
categories: [ORM]
comments: true
---

## 📚 영속성 컨텍스트?
영속성(Persistence)는 데이터를 영구적으로 저장하는 것이고, 엔티티(Entity)를 영구적으로 저장할 수 있는 환경을 말한다.
`엔티티 매니저(Entity Manager)`를 통해 영속성 컨텍스트에 접근하게 되며 `엔티티 매니저`를 통해서 엔티티를 저장, 조회 시 영속성 컨텍스트에 해당 엔티티를 저장하고 관리한다.

## 📚 영속성 컨테스트 생명주기
* 비영속(new): 영속성 컨텍스트와 무관한 상태
  * Entity 객체를 생성했지만, 영속성 컨텍스트에 저장되지 않은 상태
* 영속(managed): 영속성 컨텍스트에 저장된 상태
  * Entity Manager를 통해서 Entity를 영속성 컨텍스트에 저장한 상태
  * 해당 객체는 영속성 컨텍스트에 의해 관리된다
* 준영속(datached): 영속성 컨텍스트에 저장되었다가 분리된 상태
  * 영속성 컨텍스트가 관리하던 상태에서 엔티티를 더이상 관리하지 않는 상태
  * 준영속 상태의 특징
    * 1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떠한 기능도 동작하지 않는다.
    * 식별자 값을 가지고 있다.
* 삭제(removed): 영속성 컨텍스트에서 삭제된 상태
  * 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제

![image](https://github.com/dseoki/dseoki.github.io/assets/32925806/5d9747c4-0e80-4d27-82c6-b955bc563a5e)

```java
/* 비영속 */
// 순수 객체
Member member = new Member();

EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();

// 데이터 변경 시 트랜잭션을 시작.
transaction.begin();

/* 영속 */
// 영속성 컨텍스트에 저장
em.persist(member);

// 커밋하는 순간 DB에 INSERT 쿼리를 보냄.
transaction.commit();

/* 준영속 */
// 엔티티를 영속성 컨텍스트에서 분리
em.detach(member);
// 영속성 컨텍스트 비움 (초기화)
em.clear();
// 영속성 컨텍스트 종료
em.close();

/* 삭제 */
em.remove(member);
```

## 영속성 컨텍스트 특징

#### 1. 1차 캐시
영속성 컨텍스트는 내부적으로 1차 캐시가 존재한다.\
1차 캐시는 Map 형태로 저장된다. 키(Key)는 @Id로 매핑한 식별자이며 값(Value)은 엔티티 인스턴스이다.\
영속성 컨텍스트에 엔티티가 존재하면 DB를 조회하지 않고, 메모리에 있는 1차 캐시에서 해당 엔티티를 조회한다.

#### 2. 동일성 보장
영속성 컨텍스트는 엔티티의 동일성을 보장한다.

#### 3. 쓰기 지연
트랜잭션이 시작되면 SQL을 모아두고 트랜잭션이 종료되는 시점에 transaction.commit()이 호출되는 동시에 모아둔 쿼리를 모두 보낸다.

트랜잭션이 커밋될 떄 엔티티 매니저는 flush를 실행한다. 트랜잭션 커밋이 요청되면 엔티티 매너져는 flush를 실행하고 모여있는 쿼리들을 DB에 요청한다.

#### 4. 변경감지
별도의 update 쿼리를 사용하지 않아도 자동으로 commit시 DB의 데이터 값을 비교해 변경을 감지한다. 이를 `Dirty Checking`이라 한다.

