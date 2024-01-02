---
title: 자바 컴파일 과정
summary: class loader, jvm, compiler/interpreter에 대해서 알아보자
categories: [java]
comments: true
---

# 자바 실행 과정
```java
class Hellow {
    public static void main(String[] args) {
        System.out.println("Hellow world");
    }
}
```
위 코드와 같이 Hellow.java 파일을 생성하여 실행 시 아래와 같은 순서로 동작을 하게된다.

1. java 코드를 작성한다.
2. 자바 컴파일러가 javac.exe를 사용하여 .java로 부터 바이트 코드인 클래스 파일(.class)을 생성(컴파일)한다.
3. 클래스로더가 동적로딩을 통해 생성된 클래스 파일(바이트코드)들을 JVM 메모리 영역에 할당한다.
4. 자바 인터프리터(java.exe)로 실행한다.
5. "Hellow world"가 출력된다.

JAVA에서 가장 처음 읽어 들이는 부분의 위 코드에서 보이는 main 메서드이다. 프로그램을 실행하면 자바 인터프리터가 main 메서드를 호출하도록 되어있다.

## JVM 메모리 구조
JVM(Java Virtual Machine)은 자바 프로그램 실행환경을 만들어주는 소프트웨어이다.
JVM은 자바 실행 환경 JRE(Java Runtime Environment)에 포함 되어있다.

JVM은 메모리 용도에 따라 주요 3가지 주요 영역(method area, call stack, heap)으로 나누어 관리된다. 

1. 메서드 영역(method area)
   - 클래스의 사용되면 JVM은 .class 파일을 읽어 클래스의 정보를 이곳에 저장한다. 이 때, 그 클래스의 클래스 변수도 이 영역에 함께 생성된다.
2. 힙(heap): 언제든지 넣고 언제든지 뺄 수 있는 공간
    - 인스턴스가 생성되는 공간이다. 실행 중 생성된 모든 인스턴스는 이곳에 생성된다.
    - 인스턴스 변수들이 생성된는 공간
3. 호출스택(call stack | execution stack) : 먼저 넣은 것이 나중에 나오고 나중에 넣은게 먼저 나옴 (FILO)
    - 메서드 작업에 필요한 메모리 공간을 제공
    - 메서드가 호출되면 메모리가 할당되고, 메모리는 메서드가 작업을 수행하는 동안 지역변수들과 연산의 중간결과 등을 저장한다.
    - 메서드의 작업이 끝나면 할당되었던 메모리 공간은 반환되어 비워진다.

- 메서드가 호출되면 수행에 필요한 만큼의 메모리 스택에 할당받는다
- 메서드가 수행을 마치고나면 사용했던 메모리를 반환하고 스택에서 제거된다.
- 호출스택의 제일 위헤 있는 메서드가 현재 실행 중인 메서드이다.
- 아래에 있는 메서드가 바로 위의 메서드를 호출한 메서드이다.

#### compiler
소스코드를 기계어로 컴파일하여 실행파일을 생성하여 실행한다.
컴파일러가 개발자가 작성한 원시코드를 기계어로 변환하여 실행파일을 만든다.
런타임 상황에 모든 코드는 기계어로 변환되어 있다.

자바 소스 코드(.java)를 자바 컴파일러가 바이너리 코드(.class)로 컴파일 한다.
 
#### interfreter
코드를 한 줄씩 읽어 내려가며 실행한다.
소스코드를 기계어로 변환하는 과정 없이 한줄씩 해석하여 바로 명령어를 실행
컴파일된 바이너리 코드(.class)를 인터프리터인 JVM(Java Vertual Machine)에서 실행한다.

#### class loader
자바는 동적으로 클래스를 읽어온다. 프로그램이 실행중인 런타임에서 모든 코드가 JVM과 연결된다. 이렇게 동적으로 클래스를 로딩해주는 역할이 클래스로더이다.
클래스로더는 .class 파일을 묶어서 JVM이 운영체제로부터 할당받은 메모리 영역인 Runtime Date Area로 적재한다.

#### 참고링크
* https://sunrise-min.tistory.com/entry/%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EC%96%B8%EC%96%B4%EC%99%80-%EC%9D%B8%ED%84%B0%ED%94%84%EB%A6%AC%ED%84%B0-%EC%96%B8%EC%96%B4%EC%9D%98-%EC%B0%A8%EC%9D%B4-Java%EB%8A%94-%EC%96%B4%EB%96%A4-%EC%96%B8%EC%96%B4%EC%9D%B8%EA%B0%80
* https://velog.io/@minseojo/Java-%EC%9E%90%EB%B0%94-%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EA%B3%BC%EC%A0%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0