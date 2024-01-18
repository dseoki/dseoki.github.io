---
title: JAVA Collection
summary: JAVA Collection에 대해서 알아보자
categories: [JAVA]
comments: true
---

## Collection Freamwork
컬렉션(Collection)은 다수의 데이터, 즉 데이터 그룹을 다루는데 필요한 다양한 자료구조를 말한다.\
인터페이스와 다형성을이용한 객체지향적 설계를통해 표준화되어 사용법을 이해하기 쉽고 재사용성이 높은 코드를 작성할 수 있는 장점이 있다.

> <p style="color: #00CCFF">💡 배열을 사용하지 않고 Collection을 사용하는 이유</p>
> 배열과의 차이점은 동적 메모리 할당을 할 수 있다.
<br>

![collection](../assets/img/post/collection.png)

인터페이스 Collection은 List, Set, Queue 인터페이스가 상속하고 있는 상위의 인터페이스이고, 각 인터페이스는 다음과 같다.\
Map은 인터페이스를 상속받고 있지 않지만 Collection으로 분류한다.

| 인터페이스 | 특징 |
| --------- | ---- |
| List      | * 데이터의 순서 있음<br> * 데이터 중복 허용<br> 구현클래스: ArrayList, LInkedList, Stack, Vector 등 |
| Set       | * 데이터 순서 없음<br> * 데이터의 중복을 허용하지 않음<br> * 구현클래스: HashSet, TreeSet 등  |
| Map       | * 키(Key)와 값(Value)의 쌍으로 이루어진 데이터<br> * 데이터 순서 없음<br> * 키의 중복이 되지 않음<br> * 값의 중복 허용<br> * 구현클래스: HashMap, TreeMap, Hashtable, Properties 등 |
| Queue     | * 처음에 저장한 데이터를 가장 먼저 꺼낸다.(FIFO) |
| Stack     | * 마지막에 저장한 데이터를 가장 먼저 꺼낸다.(LIFO) |

### ArrayList
기존의 Vector를 개선한 클래스. 인터페이스 List를 구현하고 있다.
* 조회에 성능이 좋으며 순차적으로 데이터 추가/삭제가 빠르다.
* 중간에 데이터들이 추가/삭제가 일어날 경우 느리다.
* List 컬렉션을 여러 스레드에서 공유한다면 Thread safe하지 않다.

### LinkedList
* 중간에 데이터를 추가/삭제 처리시에 성능이 좋다.
* 검색이 느리다.
* 스택, 큐 등을 만들때 사용한다.

### HashSet
* 빠른 데이터 접근을 할 수 있다.
* 순서를 예측할 수 없다.
* Thread safe 하지 않다.

### TreeSet
* 정렬 방법을 지정할 수 있다. (기본 오름차순)
* Thread safe 하지 않다.
* 검색에 빠르다.
* 이진 검색 트리

### LinkedHashSet
* 입력된 순서대로 저장한다.
* Thread safe 하지 않다.

### Hashtable
* HashMap보다 느리지만 동기화를 지원한다.
* null 불가
* Thread safe 하지 않다.

### HashMap
* 데이터 추가/삭제/검색 모두 성능이 좋다.
* 중복과 순서가 허용되지 않는다.
* null 허용
  
### TreeMap
* 정렬을 가진 Map, 검색이 빠르다.
* 이진 검색 트리
