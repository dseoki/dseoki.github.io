---
title: HttpServletRequest에서 Body json 가져오기
summary: HttpServletRequest에서 Body json 가져오는 방법을 알아본다.
categories: [dev]
tags: [java]
date: 2023-03-20
comments: true
---
## Request Body 값 가져오기

REST FUL의 정의에 의하면 작업에 따라 Method가 달라야 한다고 한다. 그 이유는 멱등성 때문이다.
조회는 GET, 등록은 POST 사용한다. 그런데 작업을 하다 이러한 Controller를 보게 되었다.

```java
@ResponseBody
@RequestMapping(value = "/regist")
public ResponseEntity<String> regist(HttpServletRequest request, HttpServletResponse response, @ModelAttribute("dto") Dto dto) {
	....
}
```

REST 방식은 아니였지만 @RequestMapping 어노테이션에 `Method`를 지정해주지 않으면 GET, POST 모두가 가능하다.
문제는 다른 사람들이 등록이니 POST를 요청하였는데 값들이 dto에 매핑이 안된다. 
@ModelAttribute 어노테이션 때문이다.
해당 어노테이션은 GET에서 request parameter의 값들을 매핑을 하거나 POST에서 `multipart/form-data` 형태의 데이터를 매핑이 가능했다.
POST 전송 시 json body로 요청하면 값이 전송되지 않았다.

SpringBoot에서 @RequestBody 어노테이션을 통해 body의 json을 가져오면 DTO에 매핑이 되는데 이후 request에서 요청 값들을 확인이 불가능하였다.
@RequestBody에서 내부에서 request.getInputStream을 단 한번만 사용하여 가져올 수 있다고 한다.

그래서 아래와 같이 처리를 하였다.

```java
public static String getBody(HttpServletRequest request) throws IOException {
	StringBuilder stringBuilder = new StringBuilder();
	BufferedReader bufferedReader = null;

	try {
		InputStream inputStream = request.getInputStream();
		if (inputStream != null) {
			bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
			char[] charBuffer = new char[128];
			int bytesRead = -1;
			while ((bytesRead = bufferedReader.read(charBuffer)) > 0) {
				stringBuilder.append(charBuffer, 0, bytesRead);
			}
		}
	} catch (IOException ex) {
		throw ex;
	} finally {
		if (bufferedReader != null) {
			try {
				bufferedReader.close();
			} catch (IOException ex) {
				throw ex;
			}
		}
	}

	return stringBuilder.toString();
}
```
request로 부터 inputStream을 읽어와서 버퍼에 담아 StringBuilder에 담았다.
char 배열 크기가 128인 이유는 byte의 크기가 (-128 ~ 127) 까지 표현할 수 있기 때문인듯 하다.
이후 버퍼에서 char 배열의 크기만큼 내용을 읽어들어 StringBuilder에 쌓는다. 이후 toStirng()을 통해 문자열로 리턴받아 아래처럼 처리하였다.

```java
String bodyJson = this.getBody(request);
ObjectMapper mapper = new ObjectMapper();
dto =  mapper.readValue(bodyJson, InfoDto.class);
```
ObjectMapper class를 사용하여 문자열을 Object로 변환하는 readValue 메소드를 사용하였다.
