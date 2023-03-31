### 19. 프로토타입

- 자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어

- 자바스크립트는 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 가지고 있는 프로토타입 기반의 객체지향 프로그래밍 언어

- 자바스크립트를 이루고 있는 거의 '모든 것'이 객체
  (원시 타입의 값을 제외한 나머지 값들 : 함수, 배열, 정규 표현식 등은 모두 객체)

## 객체지향 프로그래밍

- 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차 지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임

- 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조
  (객체의 상태 데이터 : 프로퍼티, 동작 : 메서드)

## 프로토타입의 생성 시점

- 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

## 객체 생성 방식과 프로토타입의 결정

- 객체 생성 방법

  1. 객체 리터럴
  2. Object 생성자 함수
  3. 생성자 함수
  4. Object.create 메서드
  5. 클래스(ES6)

  - 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점

- 프로토타입 Person.prototype에 프로퍼티 추가하여 하위 객체가 상속받을 수 있도록 구현하기

```js
function Person(name) {
  this.name = name;
}
// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${name}`);
};

const me = new Person("Lee");
const you = new Person("Kim");

me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi My name is Kim
```

## 프로토타입 체인

```js
function Person(name) {
  this.name = name;
}
// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${name}`);
};

const me = new Person("Lee");

// hasOwnProperty는 Object.prototype의 메서드다
console.log(me.hasOwnProperty("name")); // true
```

- Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드인 hasOwnProperty를 호출할 수 있다. 이것은 me 객체가 Person.prototype 뿐만 아니라 Object.prototype도 상속받았다는 것을 의미한다.
  me 객체의 프로토타입은 Person.prototype이다.

- 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라고 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

- 프로토타입 체인 : 상속과 프로퍼티 검색을 위한 메커니즘
- 스코프 체인 : 식별자 검색을 위한 메커니즘

=> 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자 프로퍼티를 검색하는 데 사용

## 오버라이팅과 프로퍼티 섀도잉

- 오버라이딩 : 상위 클래스가 가지고 있는 하위 클래스가 재정의하여 사용하는 방식

- 오버로딩 : 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식. 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.

- 프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근해야 한다.

## instanceof 연산자

- instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다. 만약 우변의 피연산자가 함수가 아닐 경우 TypeError 발생한다.

- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

- instanceof 연산자는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

## Object.create에 의한 직접 상속

- Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.
- 첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달
- 두 번쨰 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달

- 장점

  1. new 연산자 없이도 객체를 생성할 수 있음
  2. 프로토타입을 지정하면서 객체를 생성할 수 있음
  3. 객체 리터럴에 의해 생성된 객체도 상속받을 수 있음

- ES6에서는 객체 리터럴 내부에서 **proto**접근자 프로퍼티를 사용해 직접 상속을 구현할 수 있다.

## 프로퍼티 존재 확인

# in연산자

- in연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

```js
/**
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가하는 표현식
 */
key in object;
```

```js
const person = {
  name: "Lee",
  address: "Seoul",
};
// person 객체에 name 프로퍼티가 존재한다
console.log("name" in person); // true

// person 객체에 address 프로퍼티가 존재한다
console.log("address" in person); // true

// person 객체에 age 프로퍼티가 존재하지 않는다
console.log("age" in person); // false
```

- reflect.has 메소드도 동일하게 사용할 수 있다.

# object.prototype.hasOwnProperty 메서드

- object.prototype.hasOwnProperty 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.

```js
console.log(person.hasOwnProperty("name")); //true
console.log(person.hasOwnProperty("age")); //false
```

## 프로퍼티 열거

# for...in 문

- 객체의 모든 프로퍼티를 순회하며 열거하려면 for...in문을 사용한다.
  for(변수선언문 in 객체){...}
