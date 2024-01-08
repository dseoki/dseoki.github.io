---
title: Garbage Collector
summary: GC의 구조에 대해서 알아보자
categories: [JAVA]
comments: true
---

# Garbage Collector(GC)
자동으로 메모리를 관리하여 프로그래머가 명시적으로 메모리를 해제할 필요가 없도록 한다.\
주로 Heap 메모리 영역에서 사용되지 않는 객체를 주기적으로 탐지하고 제거하는 역할을 한다.

## GC의 장단점
#### GC의 장점
C/C++ 언어에서는 GC가 없어 프로그래머가 수동으로 메모리를 할당하고 해제해야 했지만 JAVA에서는 JVM에 탑재되어 있는 GC가 메모리 관리를 해주기 때문에 메모리관리, 메모리누수 문제에 신경쓰지 않고 개발에만 집중할 수 있게 되었다.

#### GC의 단점
위와 같은 장점이 있지만 단점도 존재한다. 프로그래머가 언제 메모리가 해제 되는지 알 수 없고, GC가 동작하는 중 다른 동작을 멈추면 오버헤드가 발생한다.

## Heap 메모리
JVM에는 객체가 생성시 Heap 메모리에 할당이 된다.
([Heap 메모리](2023-12-31-java-compile.md#힙-heap-언제든지-넣고-언제든지-뺄-수-있는-공간))

#### Minor GC : 
Young 영역이다.
대부분의 객체는 생성 후 금방 Unreachable하게 되며 Heap 메모리의 Young 영역에 생성이 된다.

#### Major GC:
Old 영역이다. Young영역에서 살아남은 객체가 할당된다. Old 영역에 생성된다.

## GC의 동작방식
GC는 기본적으로 다음과 같은 동작을 진행하게 된다.
1. Stop The World
2. Mark and Sweep

#### Stop The World
JVM이 GC를 실행 시키기 위해 애플리케이션의 실행을 멈추는 작업이다.\
GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되기 때문에 GC 성능 개선 튜닝을 한다고 하면 보통 Stop The World의 시간을 줄이는 작업을 하게 된다.

#### Mark and Sweep
* Mark: 사용되는 메모리와 사용하지 않는 메모리를 식별하는 작업
* Sweep: 사용하지 않는 메모리를 제거하는 작업

Stop The World를 통해 작업이 중단되면 GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하여 사용되고 있는 메모리를 식별한다. 이후 사용되지 않는 메모리들은 제거한다.

## GC Algorithm
GC는 자동으로 메모리 관리를 해주는 것은 프로그래머에게 좋은 점이다. 그러나 GC가 동작하기 위해 Stop The World가 발생하고 애플리케이션이 중지되는 문제점이 생겼다.\
또한 Heap의 사이즈가 커지면서 애플리케이션의 지연(Suspend) 현상이 두드러지게 되었고 이를 막기 위한 다양한 Garbage Algorithm을 지원하고 있다.

### Serial GC
Serial GC의 Minor에서 Mark and Sweep을 사용한다.

