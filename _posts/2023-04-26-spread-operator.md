---
title: 전개 연산자(spread operator)
summary: 프로퍼티 확장 병합
categories: [JavaScript]
comments: true
---

## 전개 연산자(spread operator)
객체나 배열, 함수의 인수 목록 등을 확장하여 병합할 때 사용할 수 있는 `전개 연산자`를 알아봅니다.<br/>
무슨 말이냐?  `A`라는 객체와 `B`라는 객체가 있을때 두 객체를 병합하여 중복되는 값은 뒤에 오는 값으로 변경하고 하나의 객체로 새롭게 생성하는 것입니다.<br/>
전개 연산자는 `"..."`기호로 표현됩니다. 아래의 코드를 확인해봅니다.

```javascript
const objA = {x: 1, y: 2};
const objB = {y: 3, z: 4};

const margeObj = {...objA, ...objB};
console.log(margeObj); // {x: 1, y: 3, z: 4}
```

**objA**와 **objB**를 병합을 하는 코드입니다. 가장 처음 **objA** 객체를 전개 연산자를 통해 가져옵니다.
```javascript
{x: 1, y: 2}
```
**margeObj**의 두번째 매개변수로 **objB** 객체가 전개 연산자로 들어오면 **objB**에 있던 프로퍼티 `y`는 기존에 있던 `y`를 덮어씌어지게 됩니다. 그리고 **objB**에 있는 `z`는 기존에 없는 새로운 값으로 그대로 추가가 됩니다.

```javascript
{x: 1, y: 3, z: 4}
```

<br/>

전개 연산자를 이용하면 함수에 인자를 전달할 경우 배열과 같은 변수가 있을 떄 인덱스를 사용하지 않고도 접근할 수 있습니다
```javascript
function sum(a, b, c) {
	return a + b + c;
}

const arr = [1, 2, 3];
console.log(sum(...arr)); // 6
```

<br/>

### React에서 이용
react에서 useState를 사용할 경우 상태 값을 변경 시 다음과 같이 이용할 수 있습니다.
```javascript
const [data, setData] = useState({});

const dataObj = {
	name: 'John',
	age: 20
}
setData(dataObj); // 최초 data상태 저장

setData({...data, age: 31}); // {name: 'John', age: 31}
```