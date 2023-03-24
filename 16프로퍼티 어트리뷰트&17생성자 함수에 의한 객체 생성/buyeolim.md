# 16장. 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드  
이중 대괄호 \[\[...]] 로 감싼 형태로 표현 [🔗](https://262.ecma-international.org/13.0/#sec-object-internal-methods-and-internal-slots)  
자바 스크립트 엔진의 내부 로직이므로 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근, 호출할 수 있는 방법을 제공하지 않음 (일부는 간접적 접근 수단 제공)

```js
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근 불가
o.[[Prototype]] // Uncaught SyntaxError: Unexpected token '['

// 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단 제공
o.__proto__ // Object.prototype
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크럽터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의  
&nbsp;
프로퍼티의 상태

- 프로퍼티의 값(value)
- 값의 갱신 가능 여부(writable)
- 열거 가능 여부(enumerable)
- 재정의 가능 여부(configurable)

&nbsp;
프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯

- \[\[Value]]
- \[\[Writable]]
- \[\[Enumerable]]
- \[\[Configurable]]

```js
const person = {
  name: "Lee",
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스트립터 객체를 반환
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

프로퍼티 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인 가능  
Object.getOwnPropertyDescriptor 메서드 호출 시

- 첫 번째 매개변수 &rarr; 객체의 참조 전달
- 두 번째 매개변수 &rarr; 프로퍼티 키를 문자열로 전달

반환 시

- 프로퍼티 디스크립터 객체 반환

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분 가능

- **데이터 프로퍼티**
  키와 값으로 구성된 일반적인 프로퍼티
- **접근자 프로퍼티**
  자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티

데이터 프로퍼티의 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체의 프로퍼티

- \[\[Value]]  
  &rarr; value
- \[\[Writable]]  
  &rarr; writable
- \[\[Enumerable]]  
  &rarr; enumerable
- \[\[Configurable]]  
  &rarr; configurable

```js
const person = {
  name: "Lee",
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 취득
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

### 16.3.2 접근자 프로퍼티

접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티  
접근자 프로퍼티의 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체의 프로퍼티

- \[\[Get]]  
  &rarr; get
- \[\[Set]]  
  &rarr; set
- \[\[Enumerable]]  
  &rarr; enumerable
- \[\[Configurable]]  
  &rarr; configurable

접근자 함수는 getter/setter 함수라고도 부름

```js
const person = {
  // 데이터 프로퍼티
  firstName: "Ungmo",
  lastName: "Lee",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + " " + person.lastName); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수 호출
person.fullName = "Heegun Lee";
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수 호출
console.log(person.fullName); // Heegun Lee

// firstName은 데이터 프로퍼티
console.log(Object.getOwnPropertyDescriptor(person, "firstName"));
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

// fullName은 접근자 프로퍼티
console.log(Object.getOwnPropertyDescriptor(person, "fullName"));
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

## 16.4 프로퍼티 정의

프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것  
Object.defineProperty 메서드를 사용하여 프로퍼티의 어트리뷰트 정의

```js
const person = {};

// 인수: 객체의 참조, 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체
// 데이터 프로퍼티 정의
Object.defineProperty(person, "firstName", {
  value: "Ungmo",
  writable: true,
  enumerable: true,
  configurable: true,
});
Object.defineProperty(person, "lastName", {
  value: "Lee",
});

// 접근자 프로퍼티 정의
Object.defineProperty(person, "fullName", {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  set(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
  enumerable: true,
  configurable: true,
});
```

Object.defineProperty 메서드로 프로퍼티 정의 시, 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략 가능 (생략된 어트리뷰트는 기본값 적용)
|프로퍼티 디스크립터 객체의 프로퍼티|대응하는 프로퍼티 어트리뷰트|생략했을 때의 기본 값|
|:---|:---|:---|
|value|\[\[Value]]|undefined|
|get|\[\[Get]]|undefined|
|set|\[\[Set]]|undefined|
|writable|\[\[Writable]]|false|
|enumerable|\[\[Enumerable]]|false|
|configurable|\[\[Configurable]]|false|

## 16.5 객체 변경 방지

객체는 변경 가능한 값이므로 재할당 없이 직접 변경 가능  
자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공

- 객체 확장 금지 메서드 Object.preventExtensions
  - [ ] 프로퍼티 추가
  - [x] 프로퍼티 삭제
  - [x] 프로퍼티 값 읽기
  - [x] 프로퍼티 값 쓰기
  - [x] 프로퍼티 어트리뷰트 재정의
- 객체 밀봉 메서드 Object.seal
  - [ ] 프로퍼티 추가
  - [ ] 프로퍼티 삭제
  - [x] 프로퍼티 값 읽기
  - [x] 프로퍼티 값 쓰기
  - [ ] 프로퍼티 어트리뷰트 재정의
- 객체 동결 메서드 Object.freeze
  - [ ] 프로퍼티 추가
  - [ ] 프로퍼티 삭제
  - [x] 프로퍼티 값 읽기
  - [ ] 프로퍼티 값 쓰기
  - [ ] 프로퍼티 어트리뷰트 재정의

### 16.5.1 객체 확장 금지

Object.preventExtensions 메서드는 객체의 확장을 금지  
확장이 금지된 객체는 프로퍼티 추가가 금지

확장이 가능한 객체인지 여부는 Object.isExtensible 메서드로 확인

### 16.5.2 객체 밀봉

Object.seal 메서드는 객체를 밀봉  
프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지  
밀봉된 객체는 읽기와 쓰기만 가능

밀봉된 객체인지 여부는 Object.isSealed 메서드로 확인

### 16.5.3 객체 동결

Object.freeze 메서드는 객체를 동결  
프로퍼티의 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지  
동결된 객체는 읽기만 가능

동결된 객체인지 여부는 Object.isFrozen 메서드로 확인

### 16.5.4 불변 객체

이전 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못함  
따라서 Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없음  
객체의 중첩 객체까지 동결하여 변경 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 함

# 17장. 생성자 함수에 의한 객체 생성

객체는 객체 리터럴 이외에도 다양한 방법으로 생성 가능  
이번 장의 학습 목표

- 생성자 함수를 사용하여 객체를 생성하는 방식
- 객체 리터럴 및 생성자 함수를 사용하여 객체를 생성하는 방식의 장단점

## 17.1 Object 생성자 함수

생성자 함수: new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
인스턴스: 생성자 함수에 의해 생성된 객체

```js
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Lee";
person.sayHello = function () {
  console.log("Hi! My name is " + this.name);
};

console.log(person); // {name: "Lee", sayHello: ƒ}
person.sayHello(); // Hi! My name is Lee
```

자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편  
단 하나의 객체만을 생성하므로 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 비효율적

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성 함수로 동작

### 17.2.3 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할

- 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로서 동작하여 인스턴스 생성 \[필수]
- 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당) \[옵션]

new 연산자와 함께 생성자 함수를 호출 시, 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

### 17.2.4 내부 메서드 \[\[call]]과 \[\[Construct]]

함수는 객체이므로 일반 객체와 동일하게 동작 (일반 객체가 가지고 있는 내부 슬롯, 내부 메서드를 모두 보유)  
함수는 객체이지만 일반 객체와는 다름 (일반 객체는 호출 불가, 함수는 호출 가능)

함수 객체는 일반 객체가 가지고 있는 내부 슬롯, 내부 메서드 보유  
함수 동작을 위해 함수 객체만을 위한 \[\[Environment]], \[\[FormalParameters]] 등의 내부 슬롯과 \[\[Call]], \[\[Construct]] 같은 내부 메서드를 추가로 가짐

- 일반 함수로서 호출 &rarr; 함수 객체의 내부 메서드 \[\[Call]] 호출
- 생성자 함수로서 호출 &rarr; 내부 메서드 \[\[Construct]] 호출

```js
function foo() {}

foo(); // 일반 함수로서 호출 -> [[Call]] 호출
new foo(); // 생성자 함수로서 호출 -> [[Construct]] 호출
```

\[\[Call]]을 갖는 함수 객체  
&rarr; callable (호출할 수 있는 객체, 함수)  
\[\[Construct]]를 갖는 함수 객체  
&rarr; constructor (생성자 함수로서 호출할 수 있는 함수)  
\[\[Construct]] 를 갖지 않는 함수 객체  
&rarr; non-constructor (객체를 생성자 함수로서 호출할 수 없는 함수)

함수 객체는 callable이면서 constructor이거나 callable이면서 non-constructor다.

### 17.2.5 constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때, 함수 정의 방식에 따라 함수를 constructor와 non-contructor로 구분

- **constructor**: 함수 선언문, 함수 표현식, 클래스
- **non-contructor**: 메서드(ES6 메서드 축약 표현), 화살표 함수

### 17.2.6 new 연산자

new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작  
함수 객체의 내부 메서드 \[\[Construct]] 호출  
new 연산자와 함께 호출하는 함수는 constructor

new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출  
함수 객체의 내부 메서드 \[\[Call]] 호출

일반 함수와 생성자 함수에 특별한 형식적 차이 없음  
따라서 생성자 함수는 일반적으로 첫 문자르 랟문자로 기술하는 파스칼 케스로 명명하여 일반 함수와 구별할 수 있도록 노력

### 17.2.7 new.target

생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서 new.target 지원  
함수 내부에 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는 지 확인 가능

- new 연산자와 함께 생성자 함수로서 호출된 함수 내부의 new.target &rarr; 함수 자신
- new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target &rarr; undefined
