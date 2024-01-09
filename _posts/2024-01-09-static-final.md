---
title: staic, final 차이
summary: static과 fianl에 대해서 알아보자
categories: [JAVA]
comments: true
---

## JAVA static, final 차이

### static
static은 정적을 의미.
위 키워드를 가진 클래스의 멤버는 모든 인스턴스가 동일한 값을 가진다.\
컴파일 시에 하나의 메모리 공간을 사용하여 할당이 된다.\

### final
final은 상수를 의미.
변경할 수 없고 변수의 선언과 초기화가 모두 이루어져야 한다. 단. 인스턴스의 경우 생성자에서 초기화가 가능하다.

### static fianl
값을 변경할 수 없고 모든 인스턴스에 대해서 동일한 값을 가진다.\
만약 상수가 각 인스턴스에 대해서 다른 값을 가져야 한다면 static으로 선언하면 안된다.