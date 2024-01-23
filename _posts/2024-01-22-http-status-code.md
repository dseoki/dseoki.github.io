---
title: HTTP status code
summary: HTTP status code에 대해서 알아보자
categories: [dev, http]
comments: true
---

# HTTP 상태 코드

HTTP(Hypertext Transfer Protocol)는 웹 서버와 웹 클라이언트 사이에서 데이터를 주고받기 
위해 사용하는 통신 방식으로 TCP/IP 프로토콜 위에서 동작한다.

HTTP란 이름대로라면 하이퍼텍스트(Hypertext)만 전송할 수 있어 보이지만, 실제로는 HTML이나 XML과 같은 하이퍼텍스트뿐 아니라 이미지, 음성, 동영상, Javascript, PDF와 각종 문서 파일 등 컴퓨터에서 다룰 수 있는 데이터라면 무엇이든 전송할 수 있다.

서버에서의 처리 결과는 응답 메시지의 상태 라인에 있는 상태 코드(status code)를 보고 파악할 수 있다.

* 1XX: Informational(정보 제공)
  * 임시 응답으로 현재 클라이언트의 요청까지는 처리되었으니 계속 진행하라는 의미입니다. HTTP 1.1 버전부터 추가되었습니다.
* 2XX: Success(성공)
  * 클라이언트의 요청이 서버에서 성공적으로 처리되었다는 의미입니다.
* 3XX: Redirection(리다이렉션)
  * 완전한 처리를 위해서 추가 동작이 필요한 경우입니다. 주로 서버의 주소 또는 요청한 URI의 웹 문서가 이동되었으니 그 주소로 다시 시도하라는 의미입니다.
* 4XX: Client Error(클라이언트 에러)
  * 없는 페이지를 요청하는 등 클라이언트의 요청 메시지 내용이 잘못된 경우를 의미합니다.
* 5XX: Server Error(서버 에러)
  * 서버 사정으로 메시지 처리에 문제가 발생한 경우입니다. 서버의 부하, DB 처리 과정 오류, 서버에서 익셉션이 발생하는 경우를 의미합니다.


## 2XX Success (성공)
* 200	OK	성공	서버가 요청을 성공적으로 처리하였다.
* 201	Created	생성됨	요청이 처리되어서 새로운 리소스가 생성되었다.\
응답 헤더 Location에 새로운 리소스의 절대 URI를 기록합니다.
* 202	Accepted	허용됨	요청은 접수하였지만, 처리가 완료되지 않았다.\
응답 헤더의 Location, Retry-After를 참고하여 클라이언트는 다시 요청을 보냅니다.