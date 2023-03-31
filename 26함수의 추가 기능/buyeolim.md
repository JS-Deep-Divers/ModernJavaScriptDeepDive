# 26장. ES6 함수의 추가 기능

## 26.1 함수의 구분

ES6 이전의 함수는 동일한 함수라도 다양한 형태로 호출 가능

```js
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않음  
ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출 가능(callable 하면서 constructor)

ES6의 이전의 모든 함수

- 사용 목적에 따라 명확한 구분이 없어 호출 방식에 특별한 제약 없음
- 생성자 함수로 호출되지 않아도 프로토타입 객체 생성
- 혼란 야기 및 실수 유발 가능성 상승, 성능 저하

문제 해결을 위해 ES6에서의 사용 목적에 따른 함수 구분

|  ES6 함수의 구분   | constructor | prototype | &emsp;super&emsp; | arguments |
| :----------------: | :---------: | :-------: | :---------------: | :-------: |
| 일반 함수(Normal)  |      O      |     O     |         X         |     O     |
|   메서드(Method)   |      X      |     X     |         O         |     O     |
| 화살표 함수(Arrow) |      X      |     X     |         X         |     X     |

## 26.2 메서드

ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미

```js
const obj = {
  x: 1,
  // foo는 메서드
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor  
ES6 메서드는 생성자 함수로서 호출 불가

```js
new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

ES6 메서드

- 인스턴스 생성하지 않음
- prototype 프로퍼티 없음
- 프로토타입 생성하지 않음

```js
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없음
obj.foo.hasOwnProperty("prototype"); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있음
obj.bar.hasOwnProperty("prototype"); // -> true
```

참고. 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor

## 26.3 화살표 함수

화살표 함수는 function 키워드 대신 화살표(=>)를 사용하여 간략하게 함수를 정의  
내부 동작도 기존의 함수보다 간략  
콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용

### 26.3.1 화살표 함수 정의

**함수 정의**  
함수 표현식으로 정의, 호출 방식은 기존 함수와 동일

```js
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

**매개변수 선언**

- 여러 개인 경우 소괄호 () 안에 매개변수 선언
  ```js
  const arrow = (x, y) => { ... };
  ```
- 한 개인 경우 소괄호 () 생략 가능
  ```js
  const arrow = x => { ... };
  ```
- 없는 경우 소괄호 () 생략 불가
  ```js
  const arrow = () => { ... };
  ```

**함수 몸체 정의**  
함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {} 생략 가능  
함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적 반환

```js
// concise body
const power = (x) => x ** 2;
power(2); // -> 4
```

화살표 함수도 일급 객체이므로 고차 함수에 인수로 전달 가능

```js
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map((v) => v * 2); // -> [ 2, 4, 6 ]
```

### 26.3.2 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor
2. 화살표 함수는 중복된 매개변수 이름 선언 불가
3. 화살표 함수는 함수 자체의 this. arguments, super, new.target 바인딩을 갖지 않음

### 26.3.3 this

화살표 함수가 일반 함수와 구별되는 가장 큰 특징 this  
화살표 함수의 this는 일반 함수의 this와 다르게 동작

this 바인딩은 함수의 호출 방식(함수가 어떻게 호출되었는지)에 따라 동적으로 결정  
콜백 함수의 경우 고차 함수의 인수로 전달되어 고차 함수 내부에서 호출되는 중첩 함수

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // ①
    return arr.map(function (item) {
      // ②
      return this.prefix + item;
      // -> TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
```

콜백 함수 내부의 this(②), 외부 함수의 this(①)가 서로 다른 값을 가리키는 문제가 발생

이와 같은 "콜백 함수 내부의 this 문제"를 해결하기 위해 ES6에서 화살표 함수를 사용

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map((item) => this.prefix + item);
  }
}

const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
// ['-webkit-transition', '-webkit-user-select']
```

화살표 함수는 함수 자체의 this 바인딩을 갖지 않음  
따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조(**lexical this**)  
렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정

만약 화살표 함수와 화살표 함수가 중첩되어 있다면, 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조

화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 Function.prototype.call\/apply\/bind 메서드를 사용해도 화살표 함수 내부의 this 교체 불가

메서드를 화살표 함수로 정의하는 것은 피해야 함

```js
// Bad
const person = {
  name: "Lee",
  sayHi: () => console.log(`Hi ${this.name}`),
};

// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프 전역의 this가 가리키는
// 전역 객체를 가리키므로 this.name은 빈 문자열을 갖는 window.name과 동일
// 전역 객체 window에는 빌트인 프로퍼티 name이 존재
person.sayHi(); // Hi
```

메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드 사용이 바람직

```js
// Good
const person = {
  name: "Lee",
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};

person.sayHi(); // Hi Lee
```

프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제 발생  
프로퍼티를 동적 추가할 때는 ES6 메서드 정의를 사용할 수 없으므로 일반 함수를 할당

```js
// Good
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () {
  console.log(`Hi ${this.name}`);
};

const person = new Person("Lee");
person.sayHi(); // Hi Lee
```

클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아닌 인스턴스 메서드  
클래스 필드에 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드 사용

```js
// Good
class Person {
  // 클래스 필드 정의
  name = "Lee";

  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}

const person = new Person();
person.sayHi(); // Hi Lee
```

### 26.3.4 super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않음  
따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조

```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킴
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived("Lee");
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

### 26.3.5 arguments

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않음  
따라서 화살표 함수 내부에서 arguments를 참조하면 상위 스코프의 arguments를 참조  
화살표 함수 자신에게 전달된 인수 목록 확인 불가  
따라서 화살표 함수로 가변 인자 함수로 구현해야 할 때는 반드시 Rest 파라미터 사용

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세개의 점 ... 을 붙여서 정의한 매개변수  
Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받음

```js
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터
  console.log(rest); // [ 1, 2, 3, 4 ,5 ]
}

foo(1, 2, 3, 4, 5);
```

일반 매개변수와 Rest 파라미터는 함께 사용 가능  
함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당

```js
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest); // [ 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

Rest 파라미터 주의할 점

- 반드시 마지막 파라미터이어야 함
- 단 하나만 선언 가능
- 매개변수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않음

### 26.4.2 Rest 파라미터와 arguments 객체

ES5에서는 함수를 정의할 때 가변 인자 함수의 경우 arguments 객체를 활용하여 인수를 전달받음  
arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체  
함수 내부에서 지역 변수처럼 사용 가능

```js
// 매개변수의 개수를 사전에 알 수 없는 가변 인자 함수
function sum() {
  // 가변 인자 함수는 arguments 객체를 통해 인수를 전달받음
  console.log(arguments);
}

sum(1, 2); // { length: 2, '0': 1, '1': 2 }
```

배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용하려면 Function.prototype.call\/apply 메서드를 사용해 arguments 객체를 배열로 변환해야하는 번거로움 존재

ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받기 가능

```js
function sum(...args) {
  // Rest 파라미터 args에는 배열 [1, 2, 3, 4, 5]가 할당
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

함수, ES6 메서드 &rarr; Rest 파라미터, arguments 객체 모두 사용 가능  
화살표 함수 &rarr; 함수 자체의 arguments 객체 갖지 않음

## 26.5 매개변수 기본값

자바스크립트 엔진은 매개변수의 개수, 인수의 개수를 체크하지 않음  
인수가 전달되지 않은 매개변수의 값: undefined

의도치 않은 결과 발생 가능, 인수가 전달되지 않은 경우 매개변수에 기본값 할당(방어 코드) 필요

```js
function sum(x, y) {
  // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값 할당
  x = x || 0;
  y = y || 0;

  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화 간소화 가능

```js
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```
