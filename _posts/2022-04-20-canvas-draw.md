---
title: html canvas
summary: canvas를 이용하여 그림 그리기
categories: [개발공부]
tags: [HTML, JavaScript]
date: 2022-04-20
comments: true
---

## HTML \<canvas\> TAG

html의 canvas를 이용하여 싸인을 그리고 이미지로 저장하는 방법을 알아보도록 한다.



##### canvas 사용 준비

* html
  ```html
  <div class="signature">
      <canvas id="drawCanvas"></canvas>
  </div>
  ```

  canvas를 이용하기 위한 html은 위와 같이하면 끝이다.
  
* css

  ```css
  #signature {
      border: 1px solid #ccc;
      height: 200px;
      margin-top: 50px;
  }
  ```

  canvas영역을 쉽게 보기 위해 css로 선을 처리하였다.
  
* javascript
  먼저 canvas와 칠판 역할을 하는 context, 연필 역할을 하는 tool로 나눈다.

  ```javascript
  canvasDraw();
  
  var canvas, context, tool;
  function canvasDraw() {
      
  }
  ```

  화면이 로딩이 되면 가장먼저 canvasDraw 함수가 실행이 된다.

  이때, 칠판이 될 수 있는 context를 생성한다.

  ```javascript
  function canvasDraw() {
      canvas = document.getElementById('drawCanvas');
      canvas.width = 300;
      canvas.height = 200;
  
      context = canvas.getContext('2d');
      context.font = 'bold 20px sans-serif';
      context.fillStyle = '#cacaca';
      context.fillText('서명란', 120, 110);
  }
  ```

  css로 이미 영역을 잡았지만 canvas의 영역을 따로 설정해주었고, canvas의 getContext 함수를 호출하여 context에 랜더링 하였다.

  그리고 싸인을 할때 처음 서명란이 보이기 위해 위와 같이 설정을 하였다.

<img width="285" alt="image" src="https://user-images.githubusercontent.com/32925806/219293759-83761df2-3e2e-434d-8eaa-fb1800f9032f.png">



칠판이 준비되었으니 그릴 수 있는 연필을 만들어보자.

```javascript
function tool_pencil() {
    var tool = this;

    // 마우스를 누루는 순간 그리기 작업을 시작한다.
    this.mousedown = function(ev) {
    }

    // 마우스가 이동하는 동안 계속 호출하여 canvas에 line을 그린다.
    this.mousemove = function(ev) {
    }

    // 마우스를 떼면 그리기 작업을 중단한다.
    this.mouseup = function(ev) {
    }

    this.mouseout = function(ev) {
    }

}
```

canvas에 그림을 그릴때 화면을 터치하는 순간 그리는 작업을 시작하여 이동하는동안 그리고 마우스를 떼면 중단하도록 한다.
위 동작들이 이루어 질 수 있도록 canvas에 이벤트가 발생시 동작하는 함수를 만들어준다.



```javascript
// canvas 요소 내의 좌표 결정
function ev_canvas(ev) {
    if(ev.offsetX || ev.offsetX == 0) { // Opera browser
        ev._x = ev.offsetX;
        ev._y = ev.offsetY;
    } else if(ev.layerX || ev.layerX == 0) { // Firefox browser
        ev._x = ev.layerX;
        ev._y = ev.layerY;
    }

    // tool의 이벤트 핸들러 호출
    var func = tool[ev.type];
    if(func) {
        func(ev);
    }
}
```

브라우저별 화면에 대한 좌표를 측정하는 포인트를 저장한다. 이후 tool에서 이벤트가 들어오면 해당 함수를 찾아 동작을 시키도록한다.
그럼 canvas에서 ev_canvas함수가 핸들링 되도록 해보자.



```javascript
function canvasDraw() {
 ...
 
	tool = new tool_pencil();
    canvas.addEventListener('mousedown', ev_canvas, false);
    canvas.addEventListener('mousemove', ev_canvas, false);
    canvas.addEventListener('mouseup', ev_canvas, false);
    canvas.addEventListener('mouseout', ev_canvas, false);
    
    context.beginPath();
}
```

canvas에서 tool을 호출하여 각 이벤트마다 en_canvas를 랜더링시킨다. 이렇게하여 ev_canvas 내에서 좌표를 가지고서 해당 이벤트를 담당할 함수를 호출한다.

이벤트 핸들링과 동시에 그림을 그릴 수 있도록 context를 활성화 시킨다. 마지막으로 좌표를 가지고 그림을 그리고 멈추는 일을 담당하는 tool을 마무리 해본다.



```javascript
function tool_pencil() {
    var tool = this;
    this.started = false;
    this.mousein = false;

    // 마우스를 누루는 순간 그리기 작업을 시작한다.
    this.mousedown = function(ev) {
        context.moveTo(ev._x, ev._y);
        tool.started = true;
        tool.mousein = true;
    }

    // 마우스가 이동하는 동안 계속 호출하여 canvas에 line을 그린다.
    this.mousemove = function(ev) {
        if(tool.started) {
            context.lineTo(ev._x, ev._y);
            context.stroke();
        }
    }

    // 마우스를 떼면 그리기 작업을 중단한다.
    this.mouseup = function(ev) {
        if(tool.started) {
            tool.started = false;
            tool.mousein = false;
        }
    }

    this.mouseout = function(ev) {
        tool.mousein = false;
    }

}
```

* moveTo : canvas에 좌표를 입력하는 역할을한다. 
* lineTo : 라인을 연결시키는 역할을 한다.
* stroke : 라인을 그리는 역할을 한다.

moveTo를 이용하여 좌표를 잡고 lineTo로 라인들을 연결하여 stroke가 화면에 라인을 그려준다.

moveTo를 사용하지 않으면 좌표가 계속 연결이 되어 화면에 그렸다가 다른 포인트 지점에서 화면을 터치하는 순간 줄이 이어져서 그려지게 된다.

lineTo를 사용하지 않으면 라인이 생성되지 않으며, stroke로 그림을 호출하여도 포인트만 잡혀있고 선들이 없기에 그려지지 않는다.

그리고 터치가 끝나는 순간에 더 이상 마우스가 움직여도 그림이 그려지지 않도록 started를 이용하였다.



**최종 코드 소스**

<details>
<summary>펼치기</summary>
<div markdown="1">

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset='utf-8'>
	<meta http-equiv='X-UA-Compatible' content='IE=edge'>
	<title>HTML canvas 태그</title>
	<meta name='viewport' content='width=device-width, initial-scale=1'>
	<link rel='stylesheet' type='text/css' media='screen' href='main.css'>
	<script src='main.js'></script>
	<style>
		body {
			width: 300px;
			margin: 0 auto;
		}

		#signature {
			border: 1px solid #ccc;
			height: 200px;
			margin-top: 50px;
		}
	</style>
</head>
<body>
	<div id="signature">
		<canvas id="drawCanvas"></canvas>
		<img style="display: none;">
	</div>

	<!-- script -->
	<script>
		canvasDraw();

		var canvas, context, tool;
		function canvasDraw() {
			canvas = document.getElementById('drawCanvas');
			canvas.width = 300;
			canvas.height = 200;

			context = canvas.getContext('2d');
			context.font = 'bold 20px sans-serif';
			context.fillStyle = '#cacaca';
			context.fillText('서명란', 120, 110);

			// pencil tool 객체를 생성 한다.
			tool = new tool_pencil();
			canvas.addEventListener('mousedown', ev_canvas, false);
			canvas.addEventListener('mousemove', ev_canvas, false);
			canvas.addEventListener('mouseup', ev_canvas, false);
			canvas.addEventListener('mouseout', ev_canvas, false);

			context.beginPath();
		}

		function tool_pencil() {
			var tool = this;
			this.started = false;
			this.mousein = false;

			// 마우스를 누루는 순간 그리기 작업을 시작한다.
			this.mousedown = function(ev) {
				// context.moveTo(ev._x, ev._y);
				tool.started = true;
				tool.mousein = true;
			}
			
			// 마우스가 이동하는 동안 계속 호출하여 canvas에 line을 그린다.
			this.mousemove = function(ev) {
				if(tool.started) {
					context.lineTo(ev._x, ev._y);
					context.stroke();
				}
			}

			// 마우스를 떼면 그리기 작업을 중단한다.
			this.mouseup = function(ev) {
				if(tool.started) {
					tool.started = false;
					tool.mousein = false;
				}
			}

			this.mouseout = function(ev) {
				tool.mousein = false;
			}

		}

		// canvas 요소 내의 좌표 결정
		function ev_canvas(ev) {
			if(ev.offsetX || ev.offsetX == 0) { // Opera browser
				ev._x = ev.offsetX;
				ev._y = ev.offsetY;
			} else if(ev.layerX || ev.layerX == 0) { // Firefox browser
				ev._x = ev.layerX;
				ev._y = ev.layerY;
			}

			// tool의 이벤트 핸들러 호출
			var func = tool[ev.type];
			if(func) {
				func(ev);
			}
		}
	</script>
</body>
</html>
```

</div>
</details>