### 14. 전역변수의 문제점

```
전역변수 생명주기 vs 지역변수 생명주기

- 전역변수 : 애플리케이션의 생명주기
- 지역변수 : 함수의 생명주기
```

Quiz1.

```js
var x = "global";

function foo() {
  console.log(x); // (1)
  var x = "local";
}
foo();
console.log(x); // (2)
```

Quiz1.answer: (1) undefined (2) global

전역 변수는 생명 주기가 길다. 따라서 많은 단점이 존재한다. 이를 방지하기 위해 즉시 실행 함수, 네임스페이스 객체, 모듈 패턴 등의 방법이 쓰였지만, ES6 모듈을 사용하면 더는 전역 변수를 사용할 수 없다.

### 15. let, const 키워드와 블록 레벨 스코프

var 키워드로 선언된 변수는 중복 선언 가능하다.

ex.

함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수로 인정된다.

```js
var x = 1;

if (true) {
  var x = 10;
}
console.log(x); // 10
```

for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역 변수가 된다.

```js
var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

console.log(i); // 5로 i 변수의 값이 변경되어버렸다.
```

## let 키워드

```
let bar = 123;
// let 이나 const 키워드로 선언한 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared

- var 키워드로 선언한 전역 변수와 전역 함수, 그리고 암묵적 전역은 전역 객체 window의 프로퍼티가 된다.

- 하지만, let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
```

ex.

```js
let x = 1;

console.log(x); // 1
console.log(window.x); // undefined
```

## const 키워드

```
const 키워드는 let 키워드와 대체적으로 비슷하다. 차이점 위주로 알아보자.

var, let 키워드로 선언한 변수는 재할당이 가능하나, const 키워드로 선언한 변수는 재할당이 불가능하다.

const 키워드는 재할당이 불가능하므로 상수를 표현하는데 사용하기도 한다.

상수는 상태 유지, 가동성, 유지 보수의 편의를 위해 사용한다.
```

ex.

```js
let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + (preTaxPrice \* 0.1); // 0.1 의 의미가 모호하다.

console.log(afterTaxPrice); // 110
```

따라서 다음과 같이 작성할 수 있다.

```js
const TAX_RATE = 0.1;

let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + (preTaxPrice \* TAX_RATE);

console.log(afterTaxPrice); // 110
```

const 키워드로 선언된 변수에 객체를 할당한 경우 값 변경이 가능하다.

ex.

```js
const person = {
name: "Hyunsoo";
}

person.name = "WiseWater";

console.log(person); // {name: "WiseWater"}
```

종합적으로 변수 선언에서 기본적으로 const를 사용하고, 재할당이 필요한 경우에만 let을 사용하자.
** 나중에 let으로 바꿔도 되니, 모를때는 일단 const를 사용하자!! **
