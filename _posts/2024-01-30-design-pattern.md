---
title: 디자인 패턴(Singleton, Factory, Adapter)에 대해서 알아보자
summary: 디자인 패턴에 대해서 알아보자
categories: [OOP]
comments: true
---

## Design Pattern (디자인 패턴)
디자인 패턴은 개발하면서 발생하는 반복적인 문제들을 어떻게 해결할 것인지에 대한 해결 방안으로 개발을 좀 더 구조적으로 설계하기 위한 해결책

### 디자인 패턴을 사용하는 이유
1. **재사용성:** 반복적인 문제에 대한 일반적인 해결책을 제공하므로, 이를 재사용하여 유사한 상황에서 코드를 더 쉽게 작성할 수 있다.
2. **가독성:** 일정한 구조로 정리하고 명확하게 작성하여 개발자가 코드를 이해하고 유지보수하기 쉽게 만든다.
3. **유지보수성:** 코드를 쉽게 모듈화 할 수 있으며, 변경이 필요한 경우 해당 모듈만 수정하여 유지보수가 쉬워진다.
4. **확장성:** 새로운 기능을 추가하거나 변경할 때 디자인 패턴을 활용하여 기존 코드를 변경하지 않고도 새로운 기능을 통합할 수 있다.
5. **안정성과 신뢰성:** 수많은 사람들이 인정한 모범 사례로 검증된 솔루션을 제공한다.

객체지향의 특성(상속, 추상화, 캡슐화, 다형성)은 객체 지향 프로그래밍을 위한 도구이며, 설계 원칙 SOLID는 도구를 올바르게 사용하기 위한 방법으로 볼 수 있다. 그렇다면 디자인 패턴은 무엇일까?

레시피이다. 떡볶이에 우유를 넣어 로제를 만들듯이 개발에도 여러 레시피가 있고 여러 비슷한 문제를 해결하기 위한 표준같은 설계 패턴이 있다. 이것이 디자인 패턴이다.

## GoF(Gang of Four)
수 많은 디자인 패턴이 있지만, 가장 유명한 디자인 패턴은 23가지로 나누어져 있다. 크게 생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류된다. 이는 GoF(Gang of Four) 디자인 패턴이라고 불리며, 에리히 감마(Erich Gamma), 리차드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vissides) 4명의 유명한 개발자들에 의해 고안되었다

### 생성패턴(Creational Pattern)
#### 📚 Singleton(싱글톤 패턴)
하나의 클래스 인스턴스를 전역에서 접근 가능하게 하면서 해당 인스턴스가 한번만 생성되도록 보장되는 패턴이다.

<details>
<summary>싱글톤 패턴 코드 보기/숨기기</summary>
<div markdown="1">

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
</div>
</details>

**⚠ 주의 사항:** 다중 스레드 환경에서 동시에 싱글톤 인스턴스를 생성할 수 있으므로, 스레드 세이프한 구현이 필요 (e.g., 더블 체크 락 기법 사용).

#### 📚 Factory Method(팩토리 메서드 패턴)
객체를 생성하기 위한 인터페이스를 정의하고, 서브 클래스에서 어떤 클래스의 인스턴스를 생성할지 결정하는 패턴이다.

<details>
<summary>팩토리 메서드 패턴 코드 보기/숨기기</summary>
<div markdown="1">

```java
// 팩토리 메서드를 가진 인터페이스
interface Product {
    void display();
}

// 팩토리 메서드를 구현하는 서브클래스 A
class ConcreteProductA implements Product {
    @Override
    public void display() {
        System.out.println("Product A");
    }
}

// 팩토리 메서드를 구현하는 서브클래스 B
class ConcreteProductB implements Product {
    @Override
    public void display() {
        System.out.println("Product B");
    }
}

// 팩토리 메서드를 가진 Creator 인터페이스
interface Creator {
    Product createProduct();
}

// ConcreteProductA를 생성하는 ConcreteCreatorA
class ConcreteCreatorA implements Creator {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

// ConcreteProductB를 생성하는 ConcreteCreatorB
class ConcreteCreatorB implements Creator {
    @Override
    public Product createProduct() {
        return new ConcreteProductB();
    }
}
```

</div>
</details>

**⚠ 주의 사항:** 팩토리 메서드의 반환 타입을 인터페이스나 추상 클래스로 하여 클라이언트 코드와의 결합도를 낮추는 것이 좋음.

#### 📚 Abstract Factory(추상 팩토리 패턴)
관련된 객체들의 집합을 생성하는 인터페이스를 제공하며, 구체적인 팩토리 클래스를 통해 객체 생성을 추상화 하는 패턴이다.

<details>
<summary>추상 팩토리 패턴 코드 보기/숨기기</summary>
<div markdown="1">

```java
// 추상 팩토리 인터페이스
interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}

// ConcreteFactory1
class ConcreteFactory1 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

// ConcreteFactory2
class ConcreteFactory2 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA2();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB2();
    }
}
```

</div>
</details>

**⚠ 주의 사항:** 새로운 종류의 제품이 추가되면 기존 팩토리와 모든 제품을 생성하는 새로운 팩토리를 추가해야 함.

#### 📚 Builder(빌더 패턴)
복잡한 객체의 생성 과정을 단순화하고, 객체를 단계적으로 생성하며 구성하는 패턴이다.

#### 📚 Prototype(프로토타입 패턴)
객체를 복제하여 새로운 객체를 생성하는 패턴으로, 기존 객체를 템플릿으로 사용하는 패턴이다.

### 구조패턴(Structural Pattern)
#### 📚  Adapter(어댑터 패턴)
인터페이스 호환성을 제공하지 않는 클래스를 사용하기 위해 래퍼(Wrapper)를 제공하는 패턴이다.

<details>
<summary>어댑터 패턴 코드 보기/숨기기</summary>
<div markdown="1">

```java
// 기존의 인터페이스
interface LegacyInterface {
    void legacyMethod();
}

// 새로운 인터페이스
interface NewInterface {
    void newMethod();
}

// 어댑터 클래스
class Adapter implements NewInterface {
    private LegacyInterface legacyObject;

    public Adapter(LegacyInterface legacyObject) {
        this.legacyObject = legacyObject;
    }

    @Override
    public void newMethod() {
        // 기존 코드를 호출하여 새로운 인터페이스로 맞춤
        legacyObject.legacyMethod();
    }
}
```

</div>
</details>

**⚠ 주의 사항:** 어댑터 클래스가 여러 클래스와 동시에 호환되도록 설계할 경우, 다중 상속 등으로 인한 복잡성을 고려해야 함.

#### 📚 Bridge(브릿지 패턴)
추상화와 구현을 분리하여 두 가지를 독립적으로 확장할 수 있는 패턴이다.

#### 📚 Composite(컴포지트 패턴)
개별 객체와 복합 객체를 동일하게 다루어, 트리 구조의 객체를 구성하는 패턴이다.

#### 📚 Decorator(데코레이터 패턴)
객체에 동적으로 새로운 기능을 추가하여 객체를 확장할 수 있는 패턴이다.

#### 📚 Facade(퍼사드 패턴)
서브시스템을 더 쉽게 사용할 수 있도록 단순한 인터페이스를 제공하는 패턴이다.

#### 📚 Flyweight(플라이웨이트 패턴)
공유 가능한 객체를 통해 메모리 사용을 최적화하는 패턴이다.

#### 📚 Proxy(프록시 패턴)
다른 객체에 대한 대리자(Proxy)를 제공하여 접근 제어, 지연 로딩 등을 구현하는 패턴이다.

### 행위 패턴(Behavioral Pattern)
#### 📚 Observer(옵저버 패턴)
객체 간의 일대다 종속 관계를 정의하여 한 객체의 상태 변경이 다른 객체들에게 알려지도록 한다.

#### 📚 Strategy(전략 패턴)
알고리즘을 정의하고, 실행 중에 선택할 수 있게 한다.

#### 📚 Command(커맨드 패턴)
요청을 객체로 캡슐화하여 요청을 매개변수화 하고, 요청을 큐에 저장하거나 로깅하고 실행을 지연시킨다.

#### 📚 State(상태 패턴)
객체의 상태를 캡슐화하고, 상태 전환을 관리한다.

#### 📚 Chain of Responsibility(책임 연쇄 패턴)
요청을 보내는 객체와 이를 처리하는 객체를 분리하여, 다양한 처리자 중 하나가 요청을 처리한다.

#### 📚 Visitor(방문자 패턴)
객체 구조를 순회하면서 다양한 연산을 수행할 수 있게 한다.

#### 📚 Interpreter(인터프리터 패턴)
언어나 문법에 대한 해석기를 제공하여, 주어진 언어로 표현된 문제를 해결하는 패턴이다.

#### 📚 Memento(메멘토 패턴)
객체의 내부 상태를 저장하고 복원할 수 있는 기능을 제공하는 패턴이다.

#### 📚 Mediator(중재자 패턴)
객체 간의 상호 작용을 캡슐화하여, 객체 간의 직접적인 통신을 방지하는 패턴이다.

#### 📚 Template Method(템플릿 메서드 패턴)
알고리즘의 구조를 정의하면서 하위 클래스에서 각 단계의 구현을 제공하는 디자인 패턴이다.

#### 📚 Iterator(이터레이터 패턴)
컬렉션 내의 요소들에 접근하는 방법을 표준화하여 컬렉션의 내부 구조에 독립적으로 접근할 수 있는 패턴이다.