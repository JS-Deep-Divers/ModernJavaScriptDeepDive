# 10장. 객체 리터럴

## 10.1 객체란?

자바스크립트는 객체 기반의 프로그래밍 언어
원시 값을 제외한 나머지 값은 모두 객체

- 원시 (타입) 값
  단 하나의 값을 나타냄
  변경 불가능한 값
- 객체 (타입) 값
  다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조
  변경 가능한 값

객체는 0개 이상의 프로퍼티로 구성된 집합
프로퍼티는 키와 값으로 구성
자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있음
프로퍼티 값이 함수일 경우 메서드라고 부름

```js
var counter = {
  num: 0, // 프로퍼티
  increase: function () {
    // 메서드
    this.num++;
  },
};
```

- **프로퍼티**: 객체의 상태를 나타내는 값(value)
- **메서드**: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

## 10.2 객체 리터럴에 의한 객체 생성

자바스크립트는 프로토타입 기반 객체지향 언어
클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법 지원

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

리터럴: 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
객체 리터럴: 객체를 생성하기 위한 표기법
객체 리터럴은 중괄호`{...}` 내에 0개 이상의 프로퍼티를 정의

## 10.3 프로퍼티

객체는 프로퍼티의 집합
프로퍼티는 키와 값으로 구성

```js
var person = {
  // 프로퍼티 키: name, 프로퍼티 값: 'Lee'
  name: "Lee",
  // 프로퍼티 키: age, 프로퍼티 값: 20
  age: 20,
};
```

- **프로퍼티 키**: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- **프로퍼티 값**: 자바스크립트에서 사용할 수 있는 모든 값

## 10.4 메서드

자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용 가능
자바스크립트의 함수는 객체(일급 객체)
함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용 가능

- **메서드**: 프로퍼티 값이 함수일 경우

```js
var circle = {
  radiuse: 5, // <- 프로퍼티

  // 원의 지름
  getDiameter: function () {
    // <- 메서드
    return 2 * this.radius; // this는 circle을 가리킴
  },
};

console.log(circle.getDiameter()); // 10
```

## 10.5 프로퍼티 접근

프로퍼티에 접근하는 방법 두 가지

- **마침표 표기법**: 마침표 프로퍼티 접근 연산자(`.`)를 사용
- **대괄호 표기법**: 대괄호 프로퍼티 접근 연산자(`[...]`)를 사용

```js
var person = {
  name: "Lee",
};

console.log(person.name); // 마침표 표기법에 의한 프로퍼티 접근
console.log(person["name"]); // 대괄호 표기법에 의한 프로퍼티 접근
```

## 10.6 프로퍼티 값 생산

이미 존재하는 프로퍼티에 값을 할당하면 갱신

```js
var person = {
  name: "Lee",
};

person.name = "Kim";

console.log(person); // {name: "Kim"}
```

## 10.7 프로퍼티 동적 생성

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가, 프로퍼티 값 할당

```js
var person = {
  name: "Lee",
};

person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

## 10.8 프로퍼티 삭제

delete 연산자는 객체의 프로퍼티 삭제
delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이여야 함
존재하지 않는 프로퍼티를 삭제하면 에러 없이 무시

```js
var person = {
  name: "Lee",
};

person.age = 20; // 프로퍼티 동적 생성

delete person.age; // 존재하는 프로퍼티 삭제
delete person.address; // 존재하지 않는 프로퍼티 삭제 불가 (에러 무시)

console.log(person); // {name: "Lee"}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

ES6에서 객체 리터럴의 확장 기능 제공

### 10.9.1 프로퍼티 축약 표현

프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때,
프로퍼티 키를 생략 가능 (프로퍼티 키는 변수 이름으로 자동 생성)

```js
var x = 1,
  y = 2;

// ES5, {x:1, y:2}
var obj = {
  x: x,
  y: y,
};

// ES6, {x:1, y:2}
const obj = { x, y };
```

### 10.9.2 계산된 프로퍼티 이름

문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해
프로퍼티 키를 동적으로 생성 가능
단, 프로퍼티 키로 사용할 표현식을 대괄호(`[...]`)로 묶어야 함
이를 **계산된 프로퍼티 이름**이라고 부름

&nbsp;
ES5에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면
객체 리터럴 외부에서 대괄호 표기법 사용

```js
// ES5
var prefix = "prop";
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

&nbsp;
ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성 가능

```js
// ES6
const prefix = "prop";
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당

```js
// ES5
var obj = {
  name: "Lee",
  sayHi: function () {
    console.log("Hi!" + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

&nbsp;
ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현 사용 가능

```js
// ES6
const obj = {
  name: "Lee",
  // 메서드 축약 표현
  sayHi() {
    console.log("Hi!" + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

# 11장. 원시 값과 객체의 비교

자바스크립트가 제공하는 7가지 데이터 타입(숫자, 문자열 , 불리언, null, undefined, 심벌, 객체)은
크게 **원시 타입**과 **객체 타입**으로 구분
&nbsp;
원시 타입과 객체 타입은 크게 세 가지 측면에서 다름

- 원시 값은 변경 불가능한 값,
  객체는 변경 가능한 값
- 원시 값을 변수에 할당하면 변수(확보된 메모리 공간)에는 실제 값이 저장,
  객체를 변수에 할당하면 변수에는 참조 값이 저장
- 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달(**값에 의한 전달**),
  객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달(**참조에 의한 전달**)

## 11.1 원시 값

### 11.1.1 변경 불가능한 값

원시 타입 값, 즉 원시 값은 변경 불가능한 값
**변경 불가능하다는 것은 변수가 아니라 값에 대한 진술**
변수는 언제든지 재할당을 통해 변수 값을 변경(교체) 가능
상수는 단 한 번만 할당이 허용되므로 변수 값을 변경(교체) 불가
상수와 변경 불가능한 값은 다른 의미

### 11.1.2 문자열과 불변성

원시 값을 저장하려면 먼저 확보해야 하는 메모리 공간의 크기를 결정해야 함
원시 타입별로 메모리 공간의 크기가 미리 정해져 있음
문자열 타입(2bytes), 숫자 타입(8bytes) 이외의 원시 타입은 브라우저 제조사의 구현에 따라 크기가 다를 수 있음
자바스크립트는 개발자의 편의를 위해 원시 타입인 문자열 타입을 제공
자바스크립트의 문자열은 원시 타입이며 변경 불가능
변수에 새로운 문자열을 재할당하는 것은 가능

### 11.1.3 값에 의한 전달

변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수에는 할당되는 변수의 원시 값이 복사되어 전달

```js
var score = 80;

var copy = score; // copy 변수에는 score 변수의 값 80이 복사되어 할당

console.log(scroe, copy); // 80 80
console.log(score === copy); // true
```

`score` 변수와 `copy` 변수는 숫자 값 80을 갖는점은 동일
`score` 변수와 `copy` 변수의 값 80은 다른 메모리에 저장된 별개의 값
`score` 변수의 값을 변경해도 `copy` 변수의 값에는 어떠한 영향도 주지 않음

## 11.2 객체

객체는 프로퍼티의 개수가 정해져 있지 않으며, 동적으로 추가되고 삭제할 수 있음
프로퍼티 값 또한 제약이 없음
객체는 원시 값과 같이 확보해야할 메모리 공간의 크기를 사전에 정해 둘 수 없음
객체는 원시 값과 다른 방식으로 동작

### 11.2.1 변경 가능한 값

객체(참조) 타입의 값, 객체는 변경 가능한 값
객체를 할당한 변수를 참조하면 메모리에 저장되어 있는 참조 값을 통해 실체 객체에 접근
&nbsp;
객체를 할당한 변수는 재할당 없이 객체를 직접 변경 가능
재할당 없이 프로퍼티를 동적으로 추가, 프로퍼티 값 갱신, 프로퍼티 자체 삭제 가능
&nbsp;
여러 개의 식별자가 하나의 객체를 공유하게 되는 부작용 가능성 존재

### 11.2.2 참조에 의한 전달

여러 개의 식별자가 하나의 객체를 공유할 수 있어 다음과 같은 부작용 발생

```js
var person = {
  name: 'Lee';
};

var copy = person; // 참조 값을 복사(얕은 복사)
```

객체를 가리키는 변수(원본, `person`)를 다른 변수(사본, `copy`)에 할당하면 원본의 참조 값이 복사되어 전달 (**참조에 의한 전달**)
원본 `person`과 사본 `copy`는 저장된 메모리 주소는 다르지만 동일한 참조 값,
모두 동일한 객체를 가리킴
**두 개의 식별자가 하나의 객체를 공유**
원본 또는 사본 중 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받음

```js
var person = {
  name: 'Lee';
}

var copy = person;

console.log(copy === person); // true (동일한 객체 참조)

copy.name = 'Kim'; // copy를 통한 객체 변경
person.address = 'Seoul'; // person을 통한 객체 변경

// copy, person은 동일한 객체를 가리킴
// 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받음
console.log(person); // {name: "Kim", address: "Seoul"}
console.log(copy); // {name: "Kim", address: "Seoul"}
```
