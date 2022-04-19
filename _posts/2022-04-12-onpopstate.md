---
title: 뒤로가기 이벤트 window.onpopstate
summary: 뒤로가기 이벤트 window.onpopstate
categories: [JavaScript]
comments: true
---

# 뒤로가기 이벤트 window.onpopstate

작업 중 뒤로가기 이벤트를 제어야 하는 상황이 생겼다.<br/>
하나의 페이지에서 두 \<div\>를 show hide 시키며 마치 다른 페이지 처럼 보이게 하였는데 두 번째 div에서 
뒤로가기를 하면 다음과 문제가 나타났다.
<br/><br/>

<img src="https://user-images.githubusercontent.com/32925806/162911844-f6e6c6e0-0b67-41b6-9390-42a28878f55a.png" alt="image1" width=350 />

<br/>

### 문제점
page1에서 page2로 이동 시 처음 <span style="color: cyan">div#step1</span>이 보이며 다음 페이지 이동 시 <span style="color: cyan">div#step2</span>가 나타나고 뒤로가 가기 했을 경우 <span style="color: cyan">div#step1</span>을 나타내고 싶은데 page1이 나타나고 있다.

이를 해결하기 위해 window.onpopstate를 사용했는데 html5 이상에서만 지원이 되며 explorer같은 데서는 오류가 발생한다.

따라서 typeof를 사용하여 오류를 해결한다. 코드는 아래와 같다

```javascript
if(typeof(history.pushState) == 'function') {
    window.onpopstate = function() {

    }
}
```
<br/>
그 다음 <span style="color: cyan">div#step1</span> 과 <span style="color: cyan">div#step2</span>를 구분하기 위해 flag 값을 주었고 이벤트 발생 시 조건문을 통해 분기 처리했다.
<br/><br/>

```javascript
let pageFlag = 'step1';
if(typeof(history.pushState) == 'function') {
    window.onpopstate = function() {
        if(pageFlag == 'step1') {
            showStep2();
        } else if(pageFlag == 'step1') {
            showStep1();
        }
    }
}
```

그런데 이렇게만 작성하면 스크립트가 동작하지 않는다.
그 이유는 스크립트를 통해 show hide를 하면 url은 변동이 되지 않으며 브라우저 history에도 변화가 없기 때문에 <span style="color: cyan">div#step2</span>에서 뒤로가기 진행 시 page1으로 이동하게 된다.

그래서 <span style="color: cyan">div#step1</span>에서 <span style="color: cyan">div#step2</span>로 이동하는 button onclick 이벤트가 발생 시 history에 정보를 추가하였다.
```javascript
$('#step1-btn').on('click', function() {
    history.pushState(null, null, ''); // 뒤로가기 이벤트 제어하기 위해 사용
});
```

pushState는 history에 페이지 이동에 대한 정보를 기록하는 듯 하다.
변수는 state, unused, url 순이다.
* state : 직렬화가 가능한 데이터 2MiB공간을 지원하며 이보다 큰 데이터라면 localStorage나 SessionStorage를 이용해야 한다.
* unused : 브라우저 타이틀을 변경한다.
* url : url을 변경한다.


