---
title: Spring Bean Scope와 종류
summary: Spring Bean Scope와 종류에 대해서 알아보자
categories: [Spring]
comments: true
---

## Bean Scope?
`Bean`이란 "POJO(Plain, old java object): 평범하고 오래된 자바 객체" 라고 부른다.
애플리케이션의 핵심을 이루는 객체이며, Spring IoC(Inversion of Control) 컨테이너에 의해 인스턴스화, 관리, 생성된다.

`스코프(Scope)`란 빈이 존재할 수 있는 범위를 말하며, 스프링 빈은 스프링 컨테이너가 시작될 때 함께 생성되고, 스프링 컨테이너가 종료될 때까지 유지된다. 이것은 기본적으로 `싱글톤` 스코프로 생성되기 떄문이다.

## 스코프의 종류
### 1. Singleton(싱글톤):
* 디폴트 스코프. 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프이다.
* 하나의 Bean에 대해서 Spring IoC Container 내에 단 하나의 객체만 존재한다.
```java
@Component
@Scope(value = "Singleton")
public class Singleton {

}
```

### 2. prototype(프로토타입):
* 스프링 컨테이너는 프로토타입 빈의 생성과 의존 관계 주입까지만 관여하고, 더는 관리하지 않는 매우 짧은 범위의 스코프이다. 따라서 종료 메서드는 호출되지 않는다.
* 하나의 Bean에 대해서 다수의 객체가 존재할 수 있다.
```java
@Component
@Scope(value = "prototype")
public class ProtoType {

}
```

### 3. 웹 스코프:
HTTP Request 생명주기 안에 단 하나의 객체만 존재한다.
Web-aware Spring ApplicationContext 안에서만 유효하다.

* request: 웹 요청이 들어오고 나갈 때까지 유지되는 스코프
* session: 웹 세션이 생성되고 종료될 때까지 유지되는 스코프\
* application: 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프

## 스코프 사용시 주의사항
1. 스코프마다 동작 방법이 다르므로 주의해서 사용해야 한다. 
2. 특별한 Scope는 꼭 필요한 곳에 최소화해서 사용해야 한다. 무분별하게 사용하면 유지보수하기 어려워 진다.
3. Spring Bean은 설정이 없으면 Singleton Scope로 생성된다. 이때 Bean에 상태를 저장하는 코드를 작성하는 것은 동시성 문제를 유발하여 위험한 상황을 초래할 수 있다.



