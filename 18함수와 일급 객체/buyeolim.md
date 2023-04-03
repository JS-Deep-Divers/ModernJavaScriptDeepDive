# 18장. 함수와 일급 객체

## 18.1 일급 객체

일급 객체의 조건

1. 무명의 리터럴로 생성 가능(런타임에 생성 가능)
2. 변수나 자료구조(객체, 배열 등)에 저장 가능
3. 함수의 매개변수에 전달 가능
4. 함수의 반환값으로 사용 가능

자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체

```js
// 1. 함수는 무명의 리터럴로 생성 가능
// 2. 함수는 변수에 저장 가능
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성, 변수에 할당
const increase = function (num) {
  return ++num;
};
const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장 가능
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달 가능
// 4. 함수의 반환값으로 사용 가능
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수 전달 가능
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 3. 함수는 매개변수에게 함수 전달 가능
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

## 18.2 함수 객체의 프로퍼티

함수는 객체이므로 프로퍼티를 가질 수 있음  
console.dir 메서드를 사용하여 함수 객체의 내부를 살펴보면 함수 객체의 프로퍼티 확인 가능  
&rarr; arguments, caller, length, name, prototype, \_\_proto\_\_

함수 객체의 데이터 프로퍼티

- arguments
- caller
- length
- name
- prototype

함수 객체의 접근자 프로퍼티

- \_\_proto\_\_ (Object.prototype 객체의 프로퍼티를 상속받음)

### 18.2.1 arguments 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체  
arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체  
함수 내부에서 지역 변수처럼 사용 (함수 외부에서 참조 불가)

자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않음

```js
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); // 2
console.log(multiply(1, 2, 3)); // 2
```

매개변수의 개수보다 인수를 적게 전달했을 경우  
&rarr; 인수가 전달되지 않은 매개변수는 undefined로 초기화된 상태 유지
매개변수의 개수보다 인수를 더 많이 전달한 경우  
&rarr; 초과된 인수는 무시됨

arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용

### 18.2.2 caller 프로퍼티

caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티  
해당 프로퍼티는 함수 자신을 호출한 함수를 가리킴

### 18.2.3 length 프로퍼티

함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킴

```js
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

arguments 객체의 length 프로퍼티: 인자(argument)의 개수  
함수 객체의 length 프로퍼티: 매개변수(parameter)의 개수

### 18.2.4 name 프로퍼티

함수 객체의 name 프로퍼티는 함수 이름을 나타냄  
ES6에서 정식 표준  
ES5와 ES6에서 동작을 달리함

```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function () {};
// ES5 -> name 프로퍼티의 값: 빈 문자열
// ES6 -> name 프로퍼티의 값: 함수 객체를 가리키는 변수 이름
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문(Function declaration)
function bar() {}
console.log(bar.name); // bar
```

### 18.2.5 \_\_proto\_\_ 접근자 프로퍼티

모든 객체는 \[\[Prototype]]이라는 내부 슬롯을 가짐  
\[\[Prototype]] 내부슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킴  
\_\_proto\_\_ 프로퍼티는 \[\[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

### 18.2.6 prototype 프로퍼티

prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티  
일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없음

```js
// 함수 객체는 prototype 프로퍼티를 소유
(function () {}.hasOwnProperty("prototype")); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않음
({}.hasOwnProperty("prototype")); // -> false
```

해당 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴
