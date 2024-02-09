---
title: Spring AOP (Aspect Oriented Programming)
summary: Spring AOP (Aspect Oriented Programming)에 대해서 알아보자.
categories: [Spring]
comments: true
---

## Spring AOP (Aspect Oriented Programming)
AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하여 재사용하는 기법이다.\
여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다.

## Spring AOP 특징
Spring AOP는 스프링 프레임워크의 일부이므로 별도의 설정 없이도 쉽게 사용할 수 있다.\
스프링의 IoC(Inversion of Control) 컨테이너와 통합되어서 AOP 기능을 활용할 수 있다.
또한 프록시 기반으로 동작되며 다양한 시점에 모듈화된 `횡단 관심사`를 적용할 수 있다.

> #### Proxy(프록시)
> 프록시는 실제 객체에 대한 간접적인 접근을 제공하고, 추가 기능을 수행하는 객체이다. Spring AOP에서는 프록시를 사용하여 대상 객체의 호출을 가로채어 AOP 기능을 구현한다. 이를 통해 횡단 관심사를 처리하고, 성능을 최적화할 수 있다.

## 흩어진 관심사(Crosscutting Concerns)
![aop img](https://github.com/dseoki/dseoki.github.io/assets/32925806/720533d6-9e9f-453c-b5a6-075854486e51)

위 이미지에서 클래스마다 같은 색들이 코드에서 메서드라고 생각할 때, A 클래스의 주황 메서드를 수정하면 B와 C도 수정을 일일이 찾아 수정해야 한다. 이는 SOLID 원칙에 위배하여 유지보수가 어렵다.\
이렇게 코드상에서 반복적으로 사용되는 공통 부분들을 발견할 수 있는데 이것을 `흩어진 관심사(Crosscutting Concerns)`라 부른다.

## AOP 주요 용어
* **Aspect(관점):** 횡단 관심사(Cross-cutting Concern)를 모듈화한 것을 말한다. 로깅, 보안, 트랜잭션 관리 등의 관심사를 정의할 수 있다.
* **Target(대상):** Aspect를 적용할 대상 객체를 말한다. 즉, 횡단 관심사가 적용될 실제 비즈니스 로직을 가진 객체나 메서드를 말한다.
* **Advice(어드바이스):** Aspect에서 특정 조인 포인트에서 실행되는 코드를 말한다. 즉, 어드바이스는 조인 포인트에서 실행되는 횡단 관심사의 구현을 나타낸다. 스프링 AOP에서는 다양한 종류의 어드바이스를 제공한다 (Before, After, Around 등)
* **Join Point(조인 포인트):** 프로그램 실행 중에 Aspect가 적용될 수 있는 특정 지점을 말한다. 메서드 호출, 메서드 실행 전/후, 필드 접근 등이 조인 포인트의 예시이다.
* **Pointcut(포인트컷):** 조인 포인트의 패턴을 정의하는 데 사용됩니다. 즉, 어떤 조인 포인트에 Aspect를 적용할 것인지를 결정하는 역할을 한다. 일반적으로 정규 표현식이나 어노테이션을 사용하여 조인 포인트를 선택한다.

## Spring AOP 구현해보기
### maven
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### gradle
```gradle
implementation 'org.springframework.boot:spring-boot-starter-aop'
```

Spring AOP를 사용하기 위해 의존성을 추가한다.

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.MyService.*(..))")
    public void logBefore() {
        System.out.println("Before executing service method...");
    }

}
```
위 코드의 어노테이션들에 대해서 설명하면 다음과 같다.
* **@Aspect:** 해당 클래스를 Aspect로 정의한다.
* **@Component:** 해당 클래스를 스프링의 컴포턴트 스캔 대상으로 지정한다. 이를 통해 해당 클래스를 스프링 빈으로 등록할 수 있다.
* **@Before:** Advice의 종류 중 하나로, 대상 메서드가 호출되기 전에 실행된다.

포인트컷 표현식을 정의한 `execution`은 일반적으로 패키지, 클래스, 메서드 이름 및 파라미터 타입을 기준으로 조인 포인트를 지정한다.

예를들어 `execution(* com.example.service.MyService.*(..))`는 com.example.service 패키지 내의 모든 클래스의 모든 메서드를 선택한다.

`execution` 표현식 구성은 다음과 같다.
```bash
execution(modifiers-pattern? return-type-pattern declaring-type-pattern? name-pattern(param-pattern) throws-pattern?)
```
* **modifiers-pattern:** 메서드의 접근 제어자를 지정한다.
* **return-type-pattern:** 메서드의 반환 타입을 지정한다.
* **declaring-type-pattern:** 메서드를 선언한 클래스의 타입을 지정한다.
* **name-pattern:** 메서드의 이름을 지정한다.
* **param-pattern:** 메서드의 파라미터를 지정한다.
* **throws-pattern:** 던지는 예외의 타입을 지정한다.

위와 같이 경로 지정 방식이 아닌 특정 어노테이션으로 지정할 수도 있다.
```java
@Aspect
@Component
public class LoggingAspect {

    @Before("@annotation(LogBefore)")
    public void logBefore() {
        System.out.println("Before executing service method...");
    }

}
```
어노테이션을 포인트컷으로 하면 `@LogBefore` 어노테이션을 특정 메서드에 붙여주면 AOP가 적용된다.

`@Before` 어드바이스 외 다른 종류는 다음과 같다.
* **@Before:** Advice 타겟 메서드가 호출되기 전에 Advice 기능 수행
* **@After:** 타겟 메서드의 결과에 관계없이 타겟 메서드과 완료되면 Advice 기능 수행
* **@AfterRunning:** 타겟 메서드가 성공적으로 결과값을 반환 한 후에 Advice 기능 수행
* **@AfterThrowing:** 타겟 메서드가 수행 중 예외를 던지면 Advice 기능 수행
* **@Around:** Advice가 타겟 메서드를 감싸 타겟 메서드 호출 전, 후에 Advice 기능 수행

## Filter, Interceptor
Spring AOP와 비슷한 필터와 인터셉터가 있는데 실무에서 많은 사람들이 구분없이 그냥 편한것을 골라 사용하는것을 많이 보았다. 무슨 차이가 있는지 확인해보자.

### Filter
HTTP 요청과 응답의 처리 과정에서 실행되며, 주로 요청 전, 후 또는 예외 발생 시점에서 동작할 수 있으며, 주로 웹 계층에서 사용된다.\
요청의 전반적인 처리를 담당하여 인코딩 변환, 보안 검사, 로깅 등과 같은 기능을 담당한다.

### Interceptor
Filter와 비슷한 역할을 하지만 주로 컨트롤러의 호출 전, 후 또는 예외 발생 시점에서 동작할 수 있으며, 컨트롤러나 서비스 레이어에서 공통으로 처리해야 하는 기능을 모듈화하여 구현할 수 있다.

---
 즉, AOP는 비즈니스 로직에 적용되는 공통 기능을 모듈화하여 재사용성을 높이는 데 사용되고, 필터와 인터셉터는 주로 웹 애플리케이션에서 요청 처리와 관련된 공통 기능을 처리하는 데 사용된다.