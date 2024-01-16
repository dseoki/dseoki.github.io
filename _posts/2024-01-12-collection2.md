---
title: Synchronized Collection과 Concurrent Collection
summary: JAVA의 Synchronized Collection과 Concurrent Collection를 비교해보자.
categories: [JAVA]
comments: true
---

## Synchronized Collection
synchronized는 동기화라는 뜻을 가지고 있는데 동기화란 작업들 사이의 수행 시기를 맞추는 것을 말한다. Collection을 사용시 동기화가 중요한 이슈가 될 수 있는데 동기화가 되는것이 무조건 좋은건 아니고 상황에 적절히 사용할 수 있어야 한다.

Vector의 경우 add()에 synchronized 키워드가 있다.

## Concurrent Collection
java1.5 부터 등장하였고 다양한 동시성 기능을 제공한다.\
HashMap에 동기화 기능을 적용한 ConcurrentHashMap이 여기에 속한다.\ SynchronizedHashMap보다 성능이 좋은데 그 이유는 동기화 블록 범위(Scope)에 있다.\
ConcurrentHashMap은 Map 전체에 락(Lock)을 걸지 않고 여러 조각으로 나누어 부분적으로 락을 건다. 이런 특징은 멀티스레드(Multi-Thread) 환경에서 더 효율적인 성능을 보인다.

