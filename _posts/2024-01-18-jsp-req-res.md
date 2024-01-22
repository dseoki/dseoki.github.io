---
title: JSP 처리과정
summary: JSP의 처리과정을 알아보자
categories: [JAVA, JSP]
comments: true
---

# JSP(Java Server Pages) 처리과정
JPS는 서블릿 기반의 '서버 스크립트 기술'이다.\
HTML에 동적을 구현하기 위해 Java코드를 삽입하는 형식이 JSP이다.
클라이언트가 요청 시 JSP가 직접적으로 처리하지 않고 자바 소스코드로 변환 후 컴파일 하여 생성된 서블릿을 실행한다.

1. 웹 브라우저가 JSP를 요청.
2. 웹 서버가 웹 컨테이너에 전달.
3. JSP 파일을 Servlet을 통해 .java로 변환
4. .java 파일을 서블릿 .class 파일로 컴파일
5. .class 파일을 웹 컨테이너로 전달
6. 웹 컨테이너는 파일을 웹 서버로 전달

> 💡 **Servlet** 동적 웹 페이지를 만들 떄 사용되는 자바 기반의 웹 애플리케이션 프로그래밍 기술이다. 

위 과정에서 클라이언트가 서버로 데이터를 요청하는 것을 Request, 웹 서버가 응답하는 것을 Response라고 한다.

## HttpServlet
HttpServlet는 추상 클래스이다. 웹 서버와 통신 과정에 필요한 기능들을 표준화 시켜놓은 것을 상속 받아 아래처럼 코드를 작성할 수 있다.

```java
@WebServlet("/test")
public class ServletTest extends HttpServlet {
	
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
}
```

## HttpServletRequest
클라이언트가 서버에 데이터를 요청 시 요청에 대한 기능과 속성들을 가지고 있는 객체

```java
request.getCookies(); // 쿠키 정보를 가져온다.
request.getSession(); // 세션 정보를 가져온다.
request.getAttribute(key); // request 속성 값을 가져온다.
request.setAttribute(key, value); // 속성을 추가한다.
request.getParameter(key); // http 요청의 파라미터를 가져온다.
request.getParameterNames(); // http 요청의 파라미터 정보들의 키들을 가져온다.
request.getParameterValues(); // http 요청의 파라미터 값들을 가져온다.
```

## HttpServletResponse
서버에서 만든 데이터를 클라이언트에게 넘겨줄 떄 사용하는 객체

```java
response.addCookies(key); // 쿠키 추가/수정
response.getStatus(); // status 값 가져오기
response.SendRedirect("/url"); // 페이지 이동
response.getWriter(); // 쓰기를 위한 메서드
response.getOutputStream(); // 바이트 출력을 위함
```



