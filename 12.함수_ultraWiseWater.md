### 12. 함수

# 함수 리터럴

- 자바스크립트 함수는 객체 타입의 값
- 숫자 값을 숫자 리터럴로 생성하고 객체를 객체 리터럴로 생성하는 것처럼 함수도 함수 리터럴로 생성할 수 있다.

```js
var f = function add(x, y) {
  return x + y;
};
```

- 일반 객체는 호출 할 수 없지만, 함수는 호출 할 수 있다.
- 함수는 **일급 객체**

## 함수 선언문

- 함수 선언문은 함수 리터럴과 형태가 동일함
- 함수 리터럴은 함수 이름을 생략할 수 있지만, 함수 선언문은 함수 이름을 생략할 수 없다.

```js
function (x,y){
  return x + y;
};
// SyntaxError: Function statements require a function name
```

- 함수 선언문은 표현식이 아닌 문

```js
//기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석된다.
// 함수 선언문에서는 함수 이름을 생략할 수 없다.
function foo() {
  console.log("foo");
}
foo(); // foo

// 함수 리터럴을 피연산자로 사용하면 함수 선언문이 아니라 함수 리터럴 표현식으로 해석된다.
// 함수 리터럴에서는 함수 이름을 생략할 수 있다.
(function bar() {
  console.log("bar");
});
bar(); // ReferenceError: bar is not defined
```

## 함수 생성 시점과 함수 호이스팅

- *함수 선언문*으로 정의된 함수는 함수 선언문 이전에 호출할 수 있다.
- *함수 표현식*으로 정의된 함수는 함수 표현식 이전에 호출할 수 없다.

```js
//함수 참조
console.dir(add); // f add(x,y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TyperError: sub is not a function

//함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

## 매개변수

- 매개 변수보다 인수가 더 많은 경우 초과된 인수는 무시된다.

```js
function add(x, y) {
  return x + y;
}
console.log(add(2, 5, 7)); // 7
```

## 반환문

```js
function multi(x, y) {
  // return 키워드와 반환값 사이에 줄바꿈이 있으면
  return; // 세미콜론 자동 삽입 기능에 의해 세미콜론이 추가된다.
  x * y; // 무시된다.
}
console.log(multi(3, 5)); // undefined
```

## 즉시 실행 함수

- 즉시 실행 함수는 단 한번만 호출되며 다시 호출할 수 없다.

## 콜백 함수

- 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.
- 매개 변수를 통해 함수 외부에서 콜백 함수를 전달받은 함수를 고차 함수라고 한다.
