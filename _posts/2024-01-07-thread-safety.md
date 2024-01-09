---
title: Thread Safety
summary: Thread Safety
categories: [JAVA]
comments: true
---

## Thread?
프로세스 내에서 작업을 수행하는 주체이며, 모든 프로세스는 한개 이상의 스레드를 가진다.\
두개 이상의 스레드를 가지는 프로세스를 멀티스레드 프로세스(Multi-threaded Process)라고 한다.

## Thread-Safety (스레드 안전)
멀티 스레드 프로그래밍에서 어떠한 함수, 변수, 또는 객체가 여러 스레드가 동시에 접근이 이루어져도 프로그램 실행에 문제가 없는 것을 말한다.

하나의 메서드를 한 스레드가 호출하여 사용중일 떄, 다른 스레드가 그 함수를 호출하여 동시에 실행되어도 각 스레드의 함수 수행 결과가 옮바르게 나와야 한다.

## Synchronization (동기화)
한 쓰레드가 진행중인 작업을 다른 쓰레드가 간선하지 못하도록 막는 것.\

쓰레드 A가 작업하던 데이터를 도중에 쓰레드 B에게 제어권이 넘어갔을 떄, 데이터의 정보를 임시로 바꾸고 다시 쓰레드 A가 제어권을 받으면 처음 의도했던 결과와 다를 것이다.

이러한 일을 방지하고자 한 쓰레드가 특정 작업을 끝마치기 전까지 다른 쓰레드에 의해 방해 받지 않도록 하기 위해 `임계 영역(critical section)`, `lock`이라는 개념이 도입되었다.

### Lock 설정하는 방법
JAVA에서 `synchronized` 키워드를 사용하여 메서드나 블록을 동기화 할 수 있다.
또한, `Lock` 인터페이스와 구현체인 `ReentrantLock`등을 이용하여 제어가 가능하다.

```java
// synchronized 메서드 이용
public synchronized void sychronizedMethod() {
    // ...
}

// synchronized 블럭 이용
private Object obj = new Object();
public void sychronizedMethod() {
    synchronized(obj) {
        //...
    }
}

// Lock 인터페이스 이용
Lock lock = new ReentrantLock();

public void lockMethod() {
    lock.lock();
    try {
        // ...
    } finally {
        lock.unlock();
    }
}
```

### Deadlock (교착상태)
두 개 이상의 쓰레드가 서로의 락을 기다리며 무한히 대기하는 상황을 말한다.

교착상태를 발생하는 충족조건은 다음과 같다.
* 상호 배제: 최소한 하나의 스레드만 동시에 엑세스 할 수 있도록 상호 배타적이여야 한다.
* 보류 및 대기: 쓰레드는 다른 리소스가 오기를 기다리는 동안 한 리소스를 보류해야 한다.
* 선점 없음: 쓰레드가 리소스를 획득 후 리소스에 대한 잠금을 강제로 제거할 수 없다.
* 순환 대기: 각 쓰레드는 순환 방식으로 다른 쓰레드에서 리소스를 기다려야 한다.
> 자원: CPU, 메모리, 디스크, 네트워크 등\
> 리소스: 객체, 데이터 등


### ThreadLocal Class
ThreadLocal class를 사용하면 각 쓰레드가 독립적인 값을 유지할 수 있다.\
동일한 하나의 코드가 여러 쓰레드에서 동시에 실행되어도 각 쓰레드는 자신만의 데이터를 가질 수 있다.\
이를 통해 쓰레드 간의 공유 자원에 대한 문제를 피할 수 있다.

```java
private static final ThreadLocal<Integer> threadLocalValue = new ThreadLocal<>();

public void setValue(int value) {
    threadLocalValue.set(value);
}

public int getValue() {
    return threadLocalValue.get();
}
```

```java
public class MyThreadLocalExample {
    private static final ThreadLocal<String> threadLocalVariable = new ThreadLocal<>();

    public static void main(String[] args) {
        // 각 쓰레드에서 Thread Local 변수에 값 설정
        Thread thread1 = new Thread(() -> {
            threadLocalVariable.set("Value for Thread 1");
            System.out.println("Thread 1: " + threadLocalVariable.get());
        });

        Thread thread2 = new Thread(() -> {
            threadLocalVariable.set("Value for Thread 2");
            System.out.println("Thread 2: " + threadLocalVariable.get());
        });

        thread1.start();
        thread2.start();
    }
}
```


### Thread-Safety 여부 판단 방법
다음과 같이 세가지로 thread-safe한 상태인지 판단 할 수 있다.
1. 전역변수나 힙, 파일과 같이 여러 스레드가 동시에 접근할 수 있는 자원을 사용하는가?
2. 핸들과 포인터를 통한 데이터의 간접 접근이 가능한가?
3. 부수 효과를 가져오는 코드가 있는가?

### Thread-safety를 지키기 위한 4가지
* Mutual Exclution (상호 배제)
* Atomic Operation (원자 연산)
* Thread-Local Storage (스레드 지역 저장소)
* Re-Entrancy (재진입성)
