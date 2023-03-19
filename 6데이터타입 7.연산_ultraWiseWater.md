### 6장

## 템플릿 리터럴

- 문자열 표기법
- 백틱 (``) 사용
- - 같은 부호 없이 간편하게 문자열을 작성할 수 있음

ex.

```js
var first = "Hyunsoo";
var last = "Choi";

console.log(`My name is ${first}${last}.`);
```

심벌 타입

- 변경 불가능한 원시 타입
- symbol 함수를 호출해 생성, 생성된 심벌 값은 외부에 노출 되지 않으며, 다른 값과 절대 중복되지 않음

객체 타입

- 자바스크립트를 이루고 있는 거의 모든 것이 객체

```js
var foo;
console.log(type of foo); //undefined

foo = 3;
console.log(type of foo); //number

foo = "Hello";
console.log(type of foo); //string

foo = true;
console.log(type of foo); //boolean

foo = null;
console.log(type of foo); //object

foo = Symbol;
console.log(type of foo); //symbol

foo = {};
console.log(type of foo); //object

foo = [];
console.log(type of foo); //object

foo = function(){};
console.log(type of foo); //function
```

### 7장

## 삼항 조건 연산자

- 조건식 ? 조건식이 true 일 때 반환할 값 : 조건식이 false일 때 반환할 값
