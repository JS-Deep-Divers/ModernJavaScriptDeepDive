# 19장. 프로토타입

자바스크립트는 멀티 패러다임 프로그래밍 언어

- 명령형(imperative) 지원
- 함수형(functional) 지원
- 프로토타입 기반 객체지향 프로그래밍(Object Oriented Programming) 지원

자바스크립트는 프로토타입 기반의 객체 지향 프로그래밍 언어  
&rarr; 클래스 기반 객체지향 프로그래밍 언어보다 효율적, 더 강력한 객체지향 프로그래밍 능력  
또한 객체 기반의 프로그래밍 언어  
&rarr; 자바스크립트를 이루고 있는 거의 모든 것(원시 타입의 값을 제외한 나머지 값들)이 객체

## 19.1 객체지향 프로그래밍

객체지향 프로그래밍은 여러 개의 독립적 단위, 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임

실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작  
실체는 특징이나 성질을 나타내는 **속성(attribute/property)** 을 통해 인식하거나 구별 가능  
&rarr; 실체: 사람  
&rarr; 속성: 이름, 성별, 나이, 신장, 체중, 주소 등

**추상화**: 다양한 속성 중에서 프로그램에 필요한 속성만을 간추려 내어 표현하는 것

```js
const person - {
  name: 'Lee',
  address: 'Seoul'
}

console.log(person); // {name: "Lee", address: "Seoul"}
```

"이름", "주소" 속성을 갖는 person이라는 객체를 자바스크립트로 표현  
프로그래머는 이름과 주소 속성으로 표현된 객체인 person을 다른 객체와 구별하여 인식  
객체는 **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복잡적인 자료구조**

원을 객체로 만든다면, 반지름 속성을 통해 원의 지름, 둘레, 넓이를 구할 수 있음  
&rarr; 반지름은 원의 **상태를 나타내는 데이터**  
&rarr; 원의 지름, 둘레, 넓이를 구하는 것은 **동작**

```js
const circle = {
  radius: 5, // 반지름

  getDiameter() {
    return 2 * this.radius; // 원의 지름: 2r
  },
  getPerimeter() {
    return 2 * Math.PI * this.radius; // 원의 둘레: 2πr
  },
  getArea() {
    return Math.PI * this.radius ** 2; // 원의 넓이: πrr
  },
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter()); // 10
console.log(circle.getPerimeter()); // 31.41592653589793
console.log(circle.getArea()); // 78.53981633974483
```

객체지향 프로그래밍은 객체의 **상태(state)** 를 나타내는 데이터와 상태 데이터를 조작할 수 있는 **동작(behavior)** 을 하나의 논리적 단위로 묶어 생각  
객체는 **상태 데이터와 동작을 하나의 논리적 단위로 묶은 복합적인 자료구조**  
&rarr; 프로퍼티: 객체의 상테 데이터  
&rarr; 메서드: 동작

## 19.2 상속과 프로토타입

상속은 객체지향 프로그래밍의 핵심 개념  
어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

const circle1 = new Circle(1); // 반지름이 1인 인스턴스 생성
const circle2 = new Circle(2); // 반지름이 2인 인스턴스 생성

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직
console.log(circle1.getArea === circle2.getArea); // false
console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

생성자 함수는 동일한 프로퍼티 구조를 갖는 객체를 여러 개 생성할 때 유용  
Circle 생성자 함수가 생성하는 모든 객체(인스턴스)는 radius 프로퍼티와 getArea 메서드를 보유  
&rarr; radius 프로퍼티 값을 일반적으로 인스턴스마다 다름  
&rarr; getArea 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용, 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직

위 예제에서 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성 및 보유  
&rarr; 메모리 낭비, 퍼포먼스 저하

상속을 통해 불필요한 중복을 제거 가능  
**자바스크립트는 프로토타입을 기반으로 상속을 구현**

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받음
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유
console.log(circle1.getArea === circle2.getArea); // true
console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받음  
getArea 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype의 메서드로 할당  
Circle 생성자 함수가 생성하는 모든 인스턴스는 getArea 메서드를 상속받아 사용 가능

상속은 코드의 재사용이란 관점에서 매우 유용

## 19.3 프로토타입 객체

프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용  
프로토타입은 어떤 객체위 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공  
프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 사용 가능

모든 객체는 하나의 프로토타입을 보유  
\[\[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 \[\[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근 가능  
프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근 가능  
생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근 가능

### 19.3.1 \_\_proto\_\_ 접근자 프로퍼티

**모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 \[\[Prototype]] 내부 슬롯에 간접적으로 접근 가능**

#### \_\_proto\_\_는 접근자 프로퍼티

내부 슬롯은 프로퍼티가 아님  
자바스크립트는 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근, 호출할 수 있는 방법을 제공하지 않음  
\[\[Prototype]] 내부 슬롯에는 직접 접근 불가, \_\_proto\_\_ 접근자 프로퍼티를 통해 간접적으로 프로토타입에 접근 가능

#### \_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용

\_\_proto\_\_ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티  
모든 객체는 상속을 통해 Object.prototype.\_\_proto\_\_ 접근자 프로퍼티 사용 가능

#### \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함

```js
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어 지기 때문에 \_\_proto\_\_ 접근자 프로퍼티는 에러를 발생  
서로가 자신의 프로토타입(순환 참조)이 되는 비정상적인 프로토타입 체인이 만들어지면 종점이 존재하지 않기 때문에 프로토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠짐

프로토타입 체인은 단방향 링크드 리스트로 구현되어야 함

#### \_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않음

ES5까지 비표준, ES6에서 표준으로 채택

하지만 코드 내에서 \_\_proto\_\_ 접근자 프로퍼티를 직접 사용하는 것은 권장하지 않음  
모든 객체가 \_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문

### 19.3.2 함수 객체의 prototype 프로퍼티

**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴**

```js
// 함수 객체는 prototype 프로퍼티를 소유
(function () {}.hasOwnProperty("prototype")); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않음
({}.hasOwnProperty("prototype")); // false
```

prototype 프로퍼티는 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킴  
따라서 생성자 함수로서 호출할 수 없는 함수, non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않음

```js
// 화살표 함수는 non-constructor
const Person = (name) => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않음
console.log(Person.hasOwnProperty("prototype")); // false
// non-constructor는 프로토타입을 생성하지 않음
console.log(Person.prototype); // undefined
```

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 보유  
해당 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// me 객체의 생성자 함수는 Person
console.log(me.constructor === Person); // true
```

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결  
이때 constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수

```js
// obj 객체를 생성한 생성자 함수는 Object
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function
const add = new Function("a", "b", "return a + b");
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}
// me 객체를 생성한 생성자 함수는 Person
const me = new Person("Lee");
console.log(me.constructor === Person); // true
```

리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 존재

```js
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
  return a + b;
};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규 표현식 리터럴
const regexp = /is/gi;
```

리터럴 표기법에 의해 생성된 객체도 프로토타입 존재  
하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없음

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요  
따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 가짐  
프로토타입은 생성자 함수와 더불어 생성되며 constructor 프로퍼티에 의해 연결되어 있기 때문  
**프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재**

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| :----------------- | :---------- | :----------------- |
| 객체 리터럴        | Object      | Object.prototype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.prototype   |

## 19.5 프로토타입의 생성 시점

객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결  
**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성**

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수(constructor)는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성

```js
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성
console.log(Person.prototype); // {consturctor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

생성자 함수로서 호출할 수 없는 함수(non-constructor)는 프로토타입이 생성되지 않음

```js
// 화살표 함수는 non-constructor
const Person = (name) => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않음
console.log(Person.prototype);
```

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성  
모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성  
생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩

## 19.6 객체 생성 방식과 프로토타입의 결정

객체의 생성 방법

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

세부적인 객체 생성 방식의 차이는 있으나 추상 연상 OrdinaryObjectCreate에 의해 생성  
프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정  
이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype

```js
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받음
console.log(obj.constructor === Object); // true
console.log(obj.constructor("x")); // true
```

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype

```js
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받음
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식의 차이  
&rarr; 객체 리터럴 방식: 객체 리터럴 내부에 프로퍼티를 추가  
&rarr; Object 생성자 방식: 빈 객체를 생성한 이후 프로퍼티를 추가

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체

프로토타입은 객체, 따라서 일반 객체와 같이 프로토타입에도 프로퍼티 추가 및 삭제 가능  
이는 프로토타입 체인에 즉각 반영

```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
const you = new Person("Kim");

me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

## 19.7 프로토타입 체인

**프로토타입 체인**:자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 \[\[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토 타입의 프로퍼티를 순차적으로 검색  
프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘

프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototypoe  
모든 객체는 Object.prototype을 상속받음  
**프로토타입의 종점**: Object.prototype  
Object.prototype의 포로토타입, 즉 \[\[Prototype]] 내부 슬롯의 값은 null  
Object.prototype에서도 프로퍼티를 검색할 수 없는 경우 undefined 반환

자바스크립트엔진은 프로토타입 체인을 따라 프로퍼티 및 메서드 검색  
**프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘**

프로퍼티가 아닌 식별자는 스코프 체인에서 검색  
**스코프 체인은 식별자 검색을 위한 메커니즘**

스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용

## 19.8 오버라이딩과 프로퍼티 섀도잉

```js
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee");

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출. 프로토타입 메서드는 인스턴스 메서드에 의해 가려짐
me.sayHello(); // Hey! My name is Lee
```

**프로토타입 프로퍼티**: 프로토타입이 소유한 프로퍼티  
**인스턴스 프로퍼티**: 인스턴스가 소유한 프로퍼티

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아닌 인스턴스 프로퍼티로 추가

인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩  
프로토타입 메서드 sayHello는 가려짐  
**프로토타입 섀도잉**: 상속 관계에 의해 프로퍼티가 가려지는 현상

**오버라이딩**
상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식  
**오버로딩**
함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식  
자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현 가능

프로퍼티 삭제의 경우에도 프로토타입 메서드가 아닌 인스턴스 메서드가 삭제

## 19.9 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경 가능  
프로토타입은 생성자 함수 또는 인스턴스에 의해 교체 가능

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```js
const Person = (function ()) {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype() {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Personl;
}());

const me = new Person('Lee');
```

Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체  
프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없음  
construct 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티  
me 객체의 생성자 함수를 검색하면 Person이 아닌 Object

```js
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색
console.log(me.constructor === Object); // true
```

프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴

### 19.9.2 인스턴스에 의한 프로토타입의 교체

프로토타입은 생성자 함수의 prototype 프로퍼티뿐만 아니라 인스턴스의 \_\_proto\_\_ 접근자 프로퍼티(또는 Object.getPrototypeOf 메서드)를 통해 접근 가능  
이를 통해 프로토타입 교체 가능

```js
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// me 객체의 프로토타입을 parent 객체로 교체
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

생성자 함수에 의한 프로토타입의 교체와 마찬가지로 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴.

```js
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색
console.log(me.constructor === Object); // true
```

프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 번거롭기 때문에 프로토타입은 직접 교체하지 않는 것이 좋음  
상속 관계를 인위적으로 설정하려면 "직접 상속" 또는 ES6에서 도입된 클래스를 사용하는 것이 간편

## 19.10 instanceof 연산자

instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받음

```js
객체 instanceof 생성자 함수
```

우변의 피연산자가 함수가 아닌 경우 TypeError  
우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 프로토타입 체인 상에 존재하면 true, 아니면 false

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Person); // true
// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Object); // true
```

instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인

## 19.11 직접 상속

### 19.11.1 Object.create에 의한 직접 상속

Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체 생성  
첫 번째 매개변수: 생성할 객체의 프로토타입으로 지정할 객체  
두 번째 매개변수: 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체(생략 가능)

```js
/**
 * 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환
 * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
 * @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
 * @returns {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
 */
Object.create(prototype[, propertiesObject])
```

Object.create 메서드의 장점

- new 연산자가 없이도 객체를 생성 가능
- 프로토타입을 지정하면서 객체 생성 가능
- 객체 리터럴에 의해 생성된 객체도 상속 가능

### 19.11.2 객체 리터럴 내부에서 \_\_proto\_\_에 의한 직접 상속

ES6에서는 객체 리터럴 내부에서 \_\_proto\_\_ 접근자 프로퍼티를 사용하여 직접 상속 구현 가능

```js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속 가능
const obj = {
  y: 20,
  // 객체를 직접 상속받음
  // obj -> myProto -> Object.prototype -> null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 19.12 정적 프로퍼티/메서드

정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${}`);
};

// 정적 프로퍼티
Person.staticProp = `static prop`;

// 정적 메서드
Person.staticMethod = function () {
  console.log('staticMethod');
};

const me = new Person('Lee');

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출 불가
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 함
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있음  
Person 생성자 함수 객체가 소유한 프로퍼티/메서드 &rarr; 정적 프로퍼티/메서드  
정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 함조/호출 불가

## 19.13 프로퍼티 존재 확인

### 19.13.1 in 연산자

in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인.

```js
/**
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식
 */
key in object;
```

```js
const person = {
  name: "Lee",
  address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않음
console.log("age" in person); // false
// toString은 Object.prototype의 메서드
console.log("toString" in person); // true
```

person 객체에는 toString 프로퍼티가 없지만 in 연산자가 person 객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 toString 프로퍼티를 검색했기 때문에 true

ES6에서 도입된 Reflect.has 메서드는 in 연산자와 동일하게 동작

```js
const person = { name: "Lee" };

console.log(Reflect.has(person, "name")); // true
console.log(Reflect.has(person, "toString")); // true
```

### 19.13.2 Object.prototype.hasOwnProperty 메서드

Object.prototype.hasOwnProperty 메서드를 사용해 객체에 특정 프로퍼티의 존재 확인 가능

```js
const person = { name: "Lee" };

console.log(person.hasOwnProperty("name")); // true
console.log(person.hasOwnProperty("age")); // false
console.log(person.hasOwnProperty("toString")); // false
```

인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true 반환  
상속 받은 프로퍼티 키인 경우 false 반환

## 19.14 프로퍼티 열거

### 19.14.1 for ... in 문

객체의 모든 프로퍼티를 순회하며 열거하려면 for .. in 문을 사용

```js
for (변수선언문 in 객체) { ... }
```

for ... in 문은 객체의 프로퍼티 개수만큼 순회  
변수 선언문에서 선언한 변수에 프로퍼티 키를 할당

```js
const person = {
  name: "Lee",
  address: "Seoul",
};

// for ... in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당
for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

for ... in 문은 in 연산자처럼 순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거

### 19.14.2 Object.keys\/values\/entries 메서드

for ... in 문은 객체 자신의 고유 프로퍼티뿐 아니라 상속받은 프로퍼티도 열거  
Object.prototype.hasOwnProperty 메서드를 사용하여 객체 자신의 프로퍼티인지 확인하는 추가 처리가 필요  
객체 자신의 고유 프로퍼티만 열거하기 위해서는 for ... in 문보다 Object.keys\/values\/entries 메서드 사용을 권장

```js
const person = {
  name: "Lee",
  address: "Seoul",
  __proto__: { age: 20 },
};

console.log(Object.keys(person)); // ['name', 'address']
console.log(Object.values(person)); // ['Lee', 'Seoul']
console.log(Object.entries(person)); // [Array(2), Array(2)]
Object.entries(person).forEach(([key, value]) => console.log(key, value));
/*
name Lee
address Seoul
*/
```

Object.keys 메서드: 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환  
Object.values 메서드: 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환(ES8 도입)  
Object.entries 메서드: 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환(ES8 도입)
