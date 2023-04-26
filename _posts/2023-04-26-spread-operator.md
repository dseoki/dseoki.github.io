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

### 객체 이용

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

이번에는 배열과 객체 출력에서 Spread Operator를 이용해보겠습니다.
```javascript
var a = [1, 2, 3];
var b = [1, 4, 5];

console.log([...a, ...b]); // [1, 2, 3, 1, 4, 5]
console.log({...a, ...b}); // {0: 1, 1: 4, 2: 5}
```
두 배열을 전개 연산자를 이용하여 각각 배열과 객체로 출력을 해보았습니다. 배열의 경우 모든 요소를 배열 하나에 모두 추가하지만 객체의 경우 해당 인덱스를 덮어씌우게 됩니다.


### 복사
전개 연산자(Spread Operator)를 이용하면 배열이나 객체를 복사할 수도 있습니다. 먼저 아래의 코드를 보겠습니다.
```javascript
var obj1 = {a: 1, b: 2, c: 3};
var obj2 = obj1;

obj2.a = 10;
console.log(obj1); // {a: 10, b: 2, c: 3}
console.log(obj2); // {a: 10, b: 2, c: 3}
```
**obj1**을 생성 후 **obj2**에게 **obj1**을 할당한 후 두 객체를 출력하면 같은 결과가 나옵니다. **obj1**은 프로퍼티를 변경하지 않았는데도 변경이 되었습니다. 복사가 아니라 참조가 된 것입니다. Array와 Object는 그냥 할당을 하게 되면 공유하게되어 참조가 되는 것입니다. 그렇다면 복사는 어떻게 할 수 있을까요?

```javascript
var obj1 = {a: 1, b: 2, c: 3};
var obj2 = {...obj2};

obj2.a = 10;
console.log(obj1); // {a: 1, b: 2, c: 3}
console.log(obj2); // {a: 10, b: 2, c: 3}
```
위 코드와 같이 이용하면 복사가 가능합니다. Spread Operator는 기존 배열이나 객체를 새로운 배열이나 객체에 같은 데이터로 생성하는 연산자입니다.
따라서 값을 변경하여도 다른 배열이나 객체는 값이 변경되지 않습니다.

그러나 **Spread Operator**를 이용하여 복사를 하였더라도 프로퍼티가 배열이나 객체인 경우 복사가 되었더라도 내부의 값은 같은 메모리를 참조하게 됩니다.
```javascript
var obj1 = {a: 1, b: 2};
var obj2 = {c: 3};
var arr1 = [obj1, obj2]; // [{a: 1, b: 2}, {c: 3}]
var arr2 = [...arr1];

console.log(arr1 === arr2); // false
console.log(arr1[0] === arr2[0]); // true
console.log(arr1[1] === arr2[1]); // true
```
때문에 <b>전개 연산자(Spread Operator)</b>를 이용할때 혼동하지 않도록 주의하시길 바랍니다.

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