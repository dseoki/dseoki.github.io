---
title: Spring AOP (Aspect Oriented Programming)
summary: Spring AOP (Aspect Oriented Programming)에 대해서 알아보자.
categories: [Spring]
comments: true
---

## Spring AOP (Aspect Oriented Programming)
AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다.\
여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다.

코드상에서 반복적으로 사용되는 공통 부분들을 발견할 수 있는데 이것을 `흩어진 관심사(Crosscutting Concerns)`라 부른다.

흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지다.


