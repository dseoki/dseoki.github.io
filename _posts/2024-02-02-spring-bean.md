---
title: Spring Bean 주입 방법
summary: Spring Bean 주입 방법에 대해서 알아보자
categories: [Spring]
comments: true
---

## 의존성(DI) 주입 방법
스프링에서 빈(Bean) 주입은 [의존성 주입(Dependency Injection)](/2024-01/di-ioc)을 통해서 이루어진다.
주입 방법에는 3가지가 있다.
1. Field(필드) 주입
2. Setter(수정자) 주입
3. Constructor(생성자) 주입

스프링 공식 문서에서는 `생성자 주입`을 권장하고 있다.

## Field(필드) 주입
Spring 초창기 부터 현재까지 계속 사용되고 있으며, 필드에 @Autowired를 붙여서 바로 주입하는 방법이다.\
이 방법은 일반적으로 권장되지 않는다.

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```
필드 주입을 사용하면 DI 컨테이너 안에서만 동작하게 되어 순수 자바 코드로 테스트 하기 어렵다.

의존성을 주입하기 위해서는 빈 팩토리에 주입하려는 클래스의 경로를 등록 해주어야 하며 주입하려는 클래스의 명이 동일한 것이 여러개인 경우 `@Qualifier` 어노테이션을 사용하여야 한다.

## Setter(수정자) 주입
`@Autowired` 어노테이션을 통해서 setter 메서드의 파라미터에 해당 객체를 빈 팩토리에서 가져온다.

선택과 변경 가능성이 있는 의존 관계에 사용한다.

```java
@Service
public class UserService {
    
    private UserRepository userRepository;

    @Autowired
    public void setUserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

불변성을 유지하기 어렵고. 의존성이 변경될 수 있다.

## Constructor(생성자) 주입
생성자를 통해서 의존 관계를 주입받는 방법이다.\
생성자 호출 시점에 1번만 호출되는 것을 보장하며, 생성자가 1개만 존재하는 경우 @Autowired를 생략해도 자동 주입된다.

의존성이 필수적이며 변경되지 않아야 할 때 유용하다.\
`불변성(Immutability)`을 유지하기 쉽다. 한 번 생성되면 변경할 수 없는 객체를 만들 수 있다.

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired // 생성자 하나인 경우 생략가능
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

#### 불변 객체를 만들 수 없음
위 필드와 수정자 주입은 Service Bean이 만들어지고 (이 떄 null 상태 이 후에 객체를 생성하기 때문에 `fianl`을 할 수 없다.), BeanFactory에서 의존 객체를 가져와 주입하지만, 생성자 주입은 ServiceBean이 만들어지는 시점에 모든 의존 관계를 BeanFactory를 통해서 가져온다.\
`fianl` 키워드를 사용하여 `불변성`을 유지할 수 있다.

#### 순환 참조를 막을 수 있다.
AService와 BService가 서로 참조한다면 이러한 코드가 될 것이다.
```java
new AService(new BService(new AService(new BService(...))));
```
이는 Bean 생성 시점에 에러나 나타날 것이고, 정상적이지 않은 코드임을 알 수 있다.

### NullPointException 방지
Field, Setter의 경우 Service가 모두 생성 후 주입되어 호출되기 전까지 NPE를 알 수 없다.
생성자 주입은 Service가 생성 시 주입이 되기 때문에 BeanFactory를 검사하게 되고 컴파일 중에 NPE를 발견할 수 있다.
