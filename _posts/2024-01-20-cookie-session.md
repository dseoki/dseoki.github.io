---
title: Cookie와 Session
summary: 쿠키와 세션에 대해서 알아보자
categories: [dev]
comments: true
---

# 쿠키(Cookie)와 세션(Session)
쿠키와 세션은 웹 통신간 페이지가 이동되면서 유지되어야 할 정보를 저장하기 위해 사용된다.차이점이 있다면 쿠키는 웹 브라우저에 세션은 서버의 메모리(캐시)에 저장이 된다.

HTTP 프로토콜은 클라이언트가 서버에 요청을 보내고 서버는 클라이언트에 적절한 응답을 주고나면 연결(Connection)을 끊는 특성이 있다. 이를 `비연결지향(Connectionless)`라 한다.\
커넥션을 끊는 순간 클라이언트와 서버의 통신이 끝나며 **상태 정보는 유지하지 않는 특성**을 가지고 있다. 이를 `상태없음(Stateless)`라 한다.

HTTP는 이러한 특성을 보완하기 위해 `쿠키`와 `세션`을 사용하였다.\
비연결지향 특성 덕분에 서버 리소스 낭비를 줄이는것은 아주 큰 장점이지만, 통신할 때마다 새로 커넥션을 만들기 때문에 클라이언트는 상태를 유지하기 위해 어떤 절차를 거쳐야 하는 단점이 생겼다. 다른 페이지로 이동할 때마다 인증을 다시 받아야 하는 것이다.

## 쿠키(Cookie)
쿠키는 클라이언트 로컬에 저장되는 키와 값이 들어있는 데이터 파일이다.
쿠키에 유효시간을 설정하면 브라우저가 종료되더라도 정보가 유지되는 특징이 있다.

### 쿠키의 구성요소
* Name: 쿠키의 이름
* Value: 쿠키의 저장된 값
* Expires: 쿠키의 유지시간
* Domain: 쿠키가 사용되는 도메인
* Path: 쿠키를 반환할 경로

### 쿠키의 특징
* 클라이언트에 총 300개의 쿠키를 저장할 수 있다.
* 하나의 도메인 당 20개의 쿠키를 가질 수 있다.
* 하나의 쿠키는 4KB(4096byte)까지 저장할 수 있다.

쿠키는 클라이언트가 별도로 요청하지 않아도 서버에 요청(Request)시에 Request Header에 쿠키 값을 넣어 요청한다. 단 도메인 설정을 통해 지정된 도메인으로 요청시에만 쿠키 값이 전송된다.

### 사용예시
* 방문한 사이트에 로그인 시 아이디와 비밀번호 저장 여부
* 쇼핑몰 장바구니 기능
* 팝업 더 이상 보지 않기 기능

### 쿠키의 단점
* 쿠키 정보를 Http Header에 요청 시 마다 추가하여 보내기 때문에 상다한 트래픽이 발생한다.
* 결제정보등을 쿠키에 저장하였을 떄 쿠키가 유출되면 보안에 대한 문제점이 발생할 수 있다.
* 제한된 용량
* 클라이언트에서 개발자 도구 같은 툴로 수정 가능

## 세션(Session)
서버측에 정보를 저장한다. 서버에서는 클라이언트를 구분하기 위해 Session ID를 부여하여 클라이언트가 서버에 접속하여 브라우저를 종료할 때 까지 인증 상태를 유지한다.\
정보를 서버에 두기 때문에 쿠키보다 보안이 좋지만, 사용자가 많아지면 서버 메모리를 많이 차지하게 된다.

### 세션의 특징
* 각 클라이언트에 고유 ID 부여하여 클라이언트의 요구에 맞는 서비스 제공한다.
* 보안 면에서 쿠키보다 우수하다.
* 사용자가 많아지면 서버 메모리를 많이 차지한다.
* 브라우저를 닫거나 일정 시간 활동이 없을 경우에 만료된다.
* 용량에 제한이 없다.

### 세션 사용 예시
* 로그인 정보 유지와 같은 보안상 중요한 작업을 수행할 때 사용

### 세션의 단점
* 서버에 저장되므로 서버의 자원을 사용
* 대규모 트래픽이 발생하는 서비스에서 서버 부하 발생
