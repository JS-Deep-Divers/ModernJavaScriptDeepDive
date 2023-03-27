# 20장. strict mode

## 20.1 strict mode란?

```js
function foo() {
  x = 10;
}
foo();

console.log(x); // 10
```

**암묵적 전역**  
전역 스코프에도 x 변수 선언이 존재하지 않기 때문에 ReferenceError를 발생시킬 것 같지만 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성  
이때 전역 객체의 x 프로퍼티는 마치 전역변수처럼 사용 가능

개발자의 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 큼  
따라서 반드시 var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 함  
하지만 오타, 문법 지식의 미비 등 여러 이유로 실수는 언제나 발생하기 때문에 잠재적인 오류를 발생시키기 어려운 개발 환경 구성이 필요  
이를 지원하기 위해 ES5부터 strict mode(엄격 모드)가 추가  
이는 자바스크립트 언어의 문법을 조금 더 엄격히 적용하여 오류 발생 가능성이 높거나 최적화 작업에 이상이 있는 코드에 대해 명시적인 에러 발생  
ESLint 같은 린트 도구의 사용도 유사한 효과를 얻을 수 있음

## 20.2 strict mode의 적용

strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가

전역의 선두에 추가하면 스크립트 전체에 적용

```js
"use strict";

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 적용

```js
function foo() {
  "use strict";

  x = 10; // ReferenceError: x is not defined
}
foo();
```

## 20.3 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      "use strict";
    </script>
    <script>
      x = 1; // 에러 발생하지 않음
      console.log(x); // 1
    </script>
    <script>
      "use strict";

      y = 1; // ReferenceError: y is not defined
      console.log(y);
    </script>
  </body>
</html>
```

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

함수 단위로 strict mode 적용 가능  
어떤 함수는 strict mode를 적용하고 어떤 함수는 strict mode를 적용하지 않는 것은 바람직하지 않음  
모든 함수에 strict mode를 적용하는 것은 번거로움  
strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제 발생 가능성 존재

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError 발생

```js
(function () {
  "use strict";

  x = 1;
  console.log(x); // ReferenceError: x is not defined
})();
```

### 20.5.2 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생

```js
(function () {
  "use strict";

  var x = 1;
  delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
})();
```

### 20.5.3 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError 발생

```js
(function () {
  "use strict";

  // SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

### 20.5.4 with 문의 사용

with 문을 사용하면 SyntaxError 발생  
with 문은 전달된 객체를 스코프 체인에 추가  
with 문

- 장점: 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어 코드가 간결해짐
- 단점: 성능과 가동성이 떨어짐

따라서 with 문은 사용하지 않는 것이 좋음

```js
(function () {
  "use strict";

  // SyntaxError: Strict mode code may not include a with statement
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩  
생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문  
이 때 에러는 발생하지 않음

```js
(function () {
  "use strict";

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
})();
```

### 20.6.2 arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않음

```js
(function (a) {
  "use strict";
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않음
  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```

# 21장. 빌트인 객체

## 21.1 자바스크립트 객체의 분류

자바스크립트 객체는 크게 3개의 객체로 분류

- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

## 21.2 표준 빌트인 객체

자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map\/Set, WeakMap\/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여개의 표준 빌트인 객체 제공  
Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체  
&rarr; 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공  
&rarr; 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공

표준 빌트인 객체인 String, Number, Boolean, Function, Array, Date는 생성자 함수로 호출하여 인스턴스 생성 가능

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String("Lee"); // String {"Lee"}
console.log(typeof strObj); // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj); // object
```

## 21.3 원시값과 래퍼 객체

문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등의 표준 빌트인 생성자 함수가 존재

```js
const str = "hello";

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작  
원시값을 객체처럼 사용하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌림  
**래퍼 객체**: 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체  
ES6에서 새롭게 도입된 없시값인 심벌도 래퍼 객체를 생성

문자열, 숫자, 불리언, 심벌 이외의 원시값, 즉 null과 undefined는 래퍼 객체를 생성하지 않음

## 21.4 전역 객체

코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체  
어떤 객체에도 속하지 않은 최상위 객체

전역 객체는 자바스크립트 환경에 따라 지칭하는 이름이 제각각  
&rarr; 브라우저 환경: window(또는 self, this, frames)  
&rarr; Node.js 환경: global

전역 객체의 특징

- 개발자가 의도적으로 생성 불가(전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않음)
- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있음
- Object, String, Number, Boolean, Function, Array, RegExp, Date, Math, Promise 같은 모든 표준 빌트인 객체를 프로퍼티로 보유
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 가짐
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 됨
- let, const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유

### 21.4.1 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미  
주로 애플리케이션 전역에서 사용하는 값을 제공

**Infinity**  
Infinity 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 가짐

**NaN**  
NaN 프로퍼티는 숫자가 아님(Not-a-Number)을 나타내는 숫자값 NaN을 가짐  
Number.NaN 프로퍼티와 동일

**undefined**  
undefined 프로퍼티는 원시 타입 undefined를 값으로 가짐

### 21.4.2 빌트인 전역 함수

빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드

**eval**  
eval 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받음  
전달받은 문자열 코드가 표현식 &rarr; 런타임에 평가하여 값 생성  
전달받은 문자열이 표현식이 아닌 문 &rarr; 문자열 코드를 런타임에 실행  
문자열 코드가 여러 개의 문 &rarr; 모든 문 실행

**isFinite**  
전달받은 인수가 정상적인 유한수인지 검사  
유한수 &rarr; true 반환  
무한수 &rarr; false 반환  
숫자가 아닌 경우 &rarr; 숫자로 타입 변환 후 검사 수행

**isNaN**  
전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환  
전달받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입 변환 후 검사 수행

**parseFloat**  
전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환

**parseInt**  
전달받은 문자열 인수를 정수로 해석하여 반환

**encodeURI \/ decodeURI**  
encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩  
URI는 인터넷에 있는 자원을 나타내는 유일한 주소 (하위개념으로 URL, URN)  
decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩

**encodeURIComponent \/ decodeURIComponent**  
encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩  
decodeURIComponent 함수는 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주, 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩

### 21.4.3 암묵적 전역

```js
var x = 10; // 전역 변수

function foo() {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조 가능
console.log(x + y); // 30
```

foo 함수 내의 y는 선언하지 않은 식별자  
따라서 y = 20이 실행되면 참조 에러가 발생할 것 같지만 식별자 y는 마치 선언된 전역 변수처럼 동작  
선언하지 앟은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문

foo 함수 호출 시, 자바스크립트 엔진은 y 변수에 값을 할당하기 위해 먼저 스코프 체인을 통해 선언된 변수인지 확인  
foo 함수의 스코프와 전역 스코프 어디에서도 y 변수의 선언을 찾을 수 없으므로 참조 에러 발생  
하지만 자바스크립트 엔진은 y = 20을 window.y = 20으로 해석하여 전역 객체에 프로퍼티를 동적 생성  
y는 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작  
이를 **암묵적 전역**이라 함

y는 변수가 아니므로 변수 호이스팅이 발생하지 않음  
단지 프로퍼티인 y는 delete 연산자로 삭제 가능(전역 변수는 delete 연산자로 삭제 불가)

# 22장. this

## 22.1 this 키워드

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적 단위로 묶은 복합적인 자료구조

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수  
this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조 가능  
this는 자바스크립트 엔진에 의해 암묵적으로 생성, 코드 어디서든 참조 가능  
this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정

## 22.2 함수 호출 방식과 this 바인딩

this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정

함수 호출 방식

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply\/call\/bind 메서드에 의한 간접 호출

### 22.2.1 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩

```js
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩

### 22.2.2 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(,) 연산자 앞에 기술한 객체가 바인딩  
메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩

```js
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this 메서드를 호출한 객체에 바인딩
    return this.name;
  },
};

// 메서드 getName을 호출한 객체는 person
console.log(person.getName()); // Lee
```

### 22.2.3 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

const circle3 = Circle(15); // 일반 함수의 호출

console.log(circle3); // undefined
```

생성자 함수는 객체(인스턴스)를 생성하는 함수  
new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드  
이들 메서드는 모든 함수가 상속받아 사용 가능

Function.prototype.apply, Function.prototype.call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출

```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
 Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
 Function.prototype.call(thisArg[, arg1[, arg2[, ... ]]])
```

```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
```

apply와 call 메서드의 본질적인 기능은 함수 호출  
apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩

Function.prototype.bind 메서드는 apply, call 메서드와 달리 함수를 호출하지 않음  
첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환

```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출 필요
console.log(getThisBinding.bind(thisArg)()); // { a: 1 }
```

bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용

함수 호출 방식에 따른 this 바인딩의 동적 결정

| 함수 호출 방식                                               | this 바인딩                                                              |
| :----------------------------------------------------------- | :----------------------------------------------------------------------- |
| 일반 함수 호출                                               | 전역 객체                                                                |
| 메서드 호출                                                  | 메서드를 호출한 객체                                                     |
| 생성자 함수 호출                                             | 생성자 함수가 (미래에) 생성할 인스턴스                                   |
| Function.prototype.apply\/call\/bind 메서드에 의한 간접 호출 | Function.prototype.apply\/call\/bind 메서드에 첫 번째 인수로 전달한 객체 |
