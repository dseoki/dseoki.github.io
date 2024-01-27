---
title: HTTP Methods
summary: HTTP Methods에 대해서 알아보자(GET, POST, PUT, PATCH, DELETE)
categories: [dev, http]
comments: true
---

# HTTP Methods란?
HTTP(Hyper Text Transper Protocal)는 클라이언트와 서버 간의 통신을 위한 프로토콜로, 여러가지 메서드를 지원한다. 이를 지원하는 이유는 다양한 작업을 분류하여 명확한 의도를 전달함과 동시에 안전성 및 가시성을 얻을 수 있다.

### HTTP Methods는 왜 사용할까?
#### 명확한 의도 전달
HTTP Methods는 특정한 의도를 나타내기 때문에, 클라이언트에서 보내는 요청이 어떠한 동작을 수행하려는 것인지 명확하게 전달해야 한다.\
예를 들어 `DELETE`는 리소스를 삭제하고자 할 떄 사용되므로, 클라이언트가 어떤 동작을 원하는지를 명시적으로 나타낸다.

#### 멱등성(Idempotent)
멱등성이란 연산을 여러 번 수행하여도 결과가 달라지지 않는 성질을 뜻한다.\
즉, HTTP가 여러번 요청되어도 데이터베이스에 저장된 데이터는 변함이 없어야 한다.\
올바르게 구현한 `GET`, `PUT`, `DELETE` 메소드는 멱등성을 지녀야 한다.

#### 안전(Safe)
호출해도 리소스를 변경하지 않는 특성을 가진다.

#### 캐시 가능(Cacheable)
응답 결과를 서버에 캐시해서 사용해도 되는 메서드\
실무에서는 구현하기 어려워 `GET`, `HEAD`정도만 캐시 하여 사용한다.

## 주요 Method

### 1. GET
리소스를 조회하고 반환받기 위해 사용된다.\
서버에 전달 하고픈 데이터를 URL에 query(parameter, query string)을 통해 전달한다.\
보안에 취약하며 요청 시 URL 길이에 대한 제한이 있다.

### 2. POST
요청된 자원을 생성 및 프로세스 처리하기 위해 사용된다. Body로 데이터가 전송된다.\
데이터 전송에 대한 길이의 제한이 없다.

### 3. PUT
요청에 대한 자원을 수정하기 위해 사용한다.\
기존의 데이터가 있을때 같은 데이터가 있더라도 모두 새롭게 덮어쓰기 한다.

### 4. PATCH
요청된 자원을 수정한다. 단 PATCH는 해당 자원의 일부만 수정한다.

### 5. DELETE
요청한 자원을 삭제하기 위해 사용된다.

### 기타 메소드
* **HEAD:** GET과 동일하지만 response에 Body가 없고 response Code와 Head만 응답받는다.
* **OPTIONS:** 대상 리소스에 대한 통신을 설정하는 데 사용한다.
* **CONNECT:** 대상 자원으로 식별되는 서버에 대한 터널을 설정한다.
* **TRACE:** 대상 리소스에 대한 경로를 따라 메세지 루프백 테스트를 수행한다.

## HTTP Method 속성

| Method | Safe | Idempotent | Cacheable |
| ---- | :----: | :----: | :----: |
| GET | ✔ | ✔ | ✔ |
| HEAD | ✔ | ✔ | ✔ |
| POST | ❌ | ❌ | ✔ |
| PUT | ❌ | ✔ | ❌ |
| DELETE | ❌ | ✔ | ❌ |
| CONNECT | ❌ | ❌ | ❌ |
| OPTIONS | ✔ | ✔ | ❌ |
| TRACE | ✔ | ✔ | ❌ |
| PATCH | ❌ | ❌ | ✔ |
