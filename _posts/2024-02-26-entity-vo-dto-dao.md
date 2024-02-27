---
title: Entity, DTO, DAO, VO
summary: Entity, DTO, DAO, VO 개념과 차이점 및 관련 어노테이션에 대해 알아보자.
categories: [java]
comments: true
---

## 📖 Entity
Entity는 DataBase의 테이블과 매핑되는 클래스로 DB 테이블 내에 존재하는 컬럼들이 속성(필드)로 가져야 한다.\
Entity는 상속을 받거나 구현체여서는 안되며 테이블 내에 존재하지 않는 컬럼을 가져서도 안된다.

## 📖 DTO (Data Transfer Object)
데이터 전송 객체라는 의미를 가지고 있다. 주로 비동기 처리를 할 떄 사용한다.\
계층간(Controller, View, Business Layer) 데이터 교환을 위한 자바 빈즈(Java Beans)를 의미한다.

DTO는 로직을 가지지 않는 데이터 객체이고 getter/setter 메서드만 가진 클래스를 의미한다.

## 📖 DAO(Data Access Object)
DB에 데이터에 접근하기 위한 객체이다. DB에 접근하여 데이터를 삽입, 삭제, 조회 등 조작할 수 있는 기능을 수행한다.\

비즈니스 로직을 분리하여 DB와 관련한 메커니즘을 숨기기 위해 사용한다.

## 📖 VO(Value Object)
`Read-Only` 속성을 가진 객체이다. 단순히 값 타입을 표현하기 위해 불변 클래스를 만들어 사용한다.

Getter외 비즈니스 로직을 포함할 수 있으며, Setter는 가질 수 없다.\
VO의 핵심 역할은 객체의 값을 표현하는데 있기에 `equals()`와 `hashcode()`를 필수적으로 오버라이딩 해야한다.