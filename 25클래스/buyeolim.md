# 25장. 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

자바스크립트는 프로토타입 기반 객체지향 언어  
프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어

ES5 기반, 클래스 없이 생성자 함수와 프로토타입을 통한 객체지향 언어의 상속을 구현

```js
// ES5 생성자 함수
var Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log("Hi! My name is " + this.name);
  };

  // 생성자 함수 반환
  return Person;
})();

// 인스턴스 생성
var me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

ES6에서 도입된 클래스는 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 메커니즘 제시  
클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕

클래스는 생성자 함수와 매우 유사하게 동작하지만 몇 가지 차이가 존재

- 클래스를 new 연산자 없이 호출하면 에러가 발행,  
  생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출
- 클래스는 상속을 지원하는 extends와 super 키워드를 제공,  
  생성자 함수는 extends와 super 키워드를 지원하지 않음
- 클래스는 호이스팅이 발생하지 않는 것 처럼 동작,  
  함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생
- 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행, 해제 불가,  
  생성자 함수는 암묵적으로 strict mode가 지정되지 않음
- 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 \[\[Enumerable]]의 값이 false(열거되지 않음)

프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕 &rarr; 새로운 객체 생성 메커니즘

## 25.2 클래스 정의

클래스는 class 키워드를 사용하여 정의  
클래스 이름은 생성자 함수와 마찬가지로 파스칼 케이스를 사용하는 것이 일반적

```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

일급 객체로서 클래스 특징

- 무명의 리터럴로 생성 가능(런타임에 생성 가능)
- 변수나 자료구조에 저장 가능
- 함수의 매개변수에 전달 가능
- 함수의 반환값으로 사용 가능

클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드 세 가지

```js
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log("Hello!");
  }
}

// 인스턴스 생성
const me = new Person("Lee");

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

클래스와 생성자 함수의 정의 방식은 형태적인 면에서 매우 유사

## 25.3 클래스 호이스팅

클래스는 함수로 평가

```js
class Person {} // 클래스 선언문

console.log(typeof Person); // function
```

클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 런타임 이전에 먼저 평가되어 함수 객체 생성  
이때 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수(constructor)  
생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성

클래스는 클래스 정의 이전에 참조 불가

```js
console.log(Person); // ReferenceError

class Person {} // 클래스 선언문
```

클래스 선언문은 호이스팅이 발생하지 않는 것처럼 보이나 호이스팅이 발생

```js
const Person = "";
스;
{
  console.log(Person); // 호이스팅이 발생하지 않는다면 '' 출력
  // ReferenceError: Cannot access 'Person' before initialization

  class Person {} // 클래스 선언문
}
```

클래스는 let, const 키워드로 선언한 변수처럼 호이스팅  
일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작

## 25.4 인스턴스 생성

클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성

```js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

클래스는 반드시 new 연산자와 함께 호출

클래스 표현식으로 정의된 클래스의 경우 기명 클래스 표현식의 클래스 이름(MyClass)을 사용해 인스턴스를 생성하면 에러 발생

```js
const Person = class MyClass {};

const me = new Person(); // 식별자로 인스턴스 생성해야 함

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자
console.log(MyClass); // ReferenceError: MyClass is not defined

const you = new MyClass(); // ReferenceError: MyClass is not defined
```

## 25.5 메서드

클래스 몸체에는 0개 이상의 메서드만 선언 가능  
클래스 몸체에서 정의할 수 있는 메서드

- constructor(생성자)
- 프로토타입 메서드
- 정적 메서드

### 25.5.1 constructor

constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드, 이름 변경 불가

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

constructor는 메서드로 해석되는 것이 아니라 클래스로 평가되어 생성한 함수 객체 코드의 일부

constructor와 생성자 함수의 몇 가지 차이

- 클래스 내에 최대 한 개만 존재 가능
- 생략 가능
- 별도의 반환문을 갖지 않음

### 25.5.2 프로토타입 메서드

생성자 함수를 사용하여 인스턴스를 생성하는 경우

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

프로토타입 메서드를 생성하기 위해 명시적으로 프로토타입에 메서드 추가 필요

클래스 몸체에서 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원

### 25.5.3 정적 메서드

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

생성자 함수의 경우 명시적으로 생성자 함수에 메서드를 추가

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 정적 메서드
Person.sayHi = function () {
  console.log("Hi!");
};

// 정적 메서드 호출
Person.sayHi(); // Hi!
```

클래스에서 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.anme = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}

// 정적 메서드는 클래스로 호출, 인스턴스 없이도 호출 가능
Person.sayHi(); // Hi!

// 인스턴스 생성
const me = new Person("Lee");
me.sayHi(); // TypeError: me.sayHi is not a function
```

정적 메서드는 프로토타입 메서드 처럼 인스턴스로 호출하지 않고 클래스로 호출  
정적 메서드는 인스턴스로 호출할 수 없음  
인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않기 때문에 인스턴스로 클래스의 메서드를 상속받을 수 없음

### 25.5.4 정적 메서드의 프로토타입 메서드의 차이

정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다름
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조 가능

### 25.5.5 클래스에서 정의한 메서드의 특징

클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없음
3. 암묵적으로 strict mode로 실행
4. for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없음. 즉, 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는 프로퍼티 어트리뷰트 \[\[Enumerable]]의 값이 false
5. 내부 메서드 \[\[Construct]]를 갖지 않는 non-constructor. 따라서 new 연산자와 함께 호출 불가

## 25.6 클래스의 인스턴스 생성 과정

클래스는 new 연산자 없이 호출 불가

인스턴스의 생성 과정

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

```js
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
  }
}
```

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

인스턴스 프로퍼티는 constructor 내부에서 정의

```js
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public
  }
}

const me = new Person("Lee");

console.log(me); // Person {name: "Lee"}
console.log(me.name); // Lee (name은 public)
```

constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티  
ES6의 클래스는 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않음  
따라서 인스턴스 프로퍼티는 언제나 public.

### 25.7.2 접근자 프로퍼티

접근자 프로퍼티는 자체적으로는 값(\[\[Value]] 내부 슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티  
접근자 프로퍼티는 클래스에서도 사용 가능

```js
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  }
}

const me = new Person("Ungmo", "Lee");

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${this.firstName} ${this.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출
me.fullName = "Heegun Lee";
console.log(me); // {firstName: 'Heegun', lastName: 'Lee'}

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티
// 접근자 프로퍼티의 프로퍼티 어트리뷰트 get, set, enumerable, configurable
console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

접근자 프로퍼티는 자체적으로는 값을 갖고 있지 않음  
다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(getter, setter)로 구성  
getter와 setter의 이름은 인스턴스 프로퍼티처럼 사용

getter

- 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
- 메서드 이름 앞에 get 키워드를 사용해 정의
- 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용, 참조 시 내부적으로 getter 호출
- 무언가를 취득할 때 사용하으로 반드시 무언가를 반환

setter

- 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
- 메서드 앞에 set 키워드를 사용해 정의
- 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용, 할당 시 내부적으로 setter 호출
- 무언가를 프로퍼티에 할당할 때 사용하므로 반드시 매개변수 필요, 단 하나의 값만 할당 받기 때문에 단 하나의 매개변수만 선언 가능

클래스의 메서드는 기본적으로 프로토타입 메서드  
따라서 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티

### 25.7.3 클래스 필드 정의 제안

클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어

자바스크립트의 클래스

- 몸체에는 메서드만 선언 가능
- 인스턴스 프로퍼티를 선언, 초기화하려면 반드시 constructor 내부에서 this에 프로퍼티를 추가
- 인스턴스 프로퍼티를 참조하려면 반드시 this를 사용해서 참조

하지만 아래의 코드는 최신 브라우저, Node.js에서 실행하면 정상 작동

```js
class Person {
  // 클래스 필드 정의
  name = "Lee";
}

const me = new Person("Lee");
console.log(me); // Person {name: 'Lee'}
```

자바스크립트에서 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 클래스 필드 처럼 정의할 수 있는 새로운 표준 사양[🔗](https://github.com/tc39/proposal-class-fields)이 TC39[🔗](https://tc39.es/)로 제안되어 있는 상태  
클래스 몸체에서 클래스 필드를 정의할 수 있는 클래스 필드 정의 제안은 아직 ECMAScript의 정식 표준 사양으로 승급되지 않음  
최신 브라우저, Node.js는 이 제안을 선제적으로 미리 구현, 따라서 클래스 필드를 클래스 몸체에 정의 가능  
ES2022에 등재[🔗](https://github.com/tc39/proposals/blob/main/finished-proposals.md)

주의사항

- 클래스 몸체에서 클래스 필드를 정의하는 경우, this에 클래스 필드를 바인딩해서는 안 됨(this는 클래스의 construoctor와 메서드 내에서만 유효)
- 클래스 필드를 참조하는 경우 this를 반드시 사용해야 함
- 클래스 필드에 초기값을 할당하지 않으면 undefined
- 인스턴스 생성 시, 외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면, constructor에서 클래스 필드를 초기화
- 클래스 필드에 함수를 할당하는 경우 함수는 프로토타입 메서드가 아닌 인스턴스 메서드(모든 클래스 필드는 인스턴스 프로퍼티), 따라서 클래스 필드에 함수를 할당하는 것은 권장하지 않음

### 25.7.4 private 필드 정의 제안

자바스크립트는 캡슐화를 완전하게 지원하지 않음  
ES6의 클래스 또한 생성자 함수와 마찬가지로 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않음  
따라서 인스턴스 프로퍼티는 언제나 public

이 또한 private 필드를 정의할 수 있는 새로운 표준 사양[🔗](https://github.com/tc39/proposal-class-fields#private-fields)이 제안되어 있음  
최신 브라우저, Node.js 또한 이미 구현  
ES2022에 등재[🔗](https://github.com/tc39/proposals/blob/main/finished-proposals.md)

```js
class Person {
  // private 필드 정의
  #name = "";

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person("Lee");

// private 필드 #name은 클래스 외부에서 참조할 수 없음
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

|         접근 가능성         | public | private |
| :-------------------------: | :----: | :-----: |
|         클래스 내부         |   O    |    O    |
|      자식 클래스 내부       |   O    |    X    |
| 클래스 인스턴스를 통합 접근 |   O    |    X    |

접근자 프로퍼티를 통한 간접적 접근 방법은 유효

```js
class Person {
  // private 필드 정의
  #name = "";

  constructor(name) {
    this.#name = name;
  }

  // name은 접근자 프로퍼티
  get name() {
    // private 필드를 참조하여 trim한 다음 반환
    return this.#name.trim();
  }
}

const me = new Person(" Lee ");
console.log(me.name); // Lee
```

private 필드는 반드시 클래스 몸체에 정의

### 25.7.5 static 필드 정의 제안

클래스에는 static 키워드를 사용하여 정적 메서드 정의 가능  
static 키워드를 사용하여 정적 필드 정의 불가

static public 필드, static private 메서드를 정의할 수 있는 새로운 표준 사양[🔗](https://github.com/tc39/proposal-static-class-features)이 제안되어 있음  
최신 브라우저, Node.js에 이미 구현
ES2022에 등재[🔗](https://github.com/tc39/proposals/blob/main/finished-proposals.md)

```js
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

## 25.8 상속에 의한 클래스 확장

### 25.8.1 클래스 상속과 생성자 함수 상속

상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의

상속을 통해 Animal 클래스를 확장한 Bird 클래스 구현

```js
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() {
    return "eat";
  }

  move() {
    return "move";
  }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() {
    return "fly";
  }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird { age: 1, weight: 5 }
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드가 기본적으로 제공  
생성자 함수는 클래스와 같이 상속을 통해 다른 생성자 함수를 확장할 수 있는 문법이 제공되지 않음

### 25.8.2 extends 키워드

상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의

```js
// 수퍼(베이스/부모) 클래스
class Base {}

// 서브(파생/자식) 클래스
class Derived extends Base {}
```

서브클래스(파생 클래스): 상속을 통해 확장된 클래스  
수퍼클래스(부모 클래스): 서브클래스에게 상속된 클래스

### 25.8.3 동적 상속

extends 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스 확장 가능(extends 키워드 앞에는 반드시 클래스)

```js
// 생성자 함수
function Base(a) {
  this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```

extends 키워드 다음에는 클래스뿐만이 아니라 \[\[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있음

### 25.8.4 서브클래스의 constructor

클래스에서 constructor를 생략하면 클래스에 비어 있는 constructor가 암묵적으로 정의  
서브 클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의

```js
constructor(...args) { super(...args); }
```

super()는 수퍼클래스의 constructor(super-constructor)를 호출하여 인스턴스를 생성

### 25.8.5 super 키워드

super 키워드는 함수처럼 호출, this 같이 식별자처럼 참조 가능한 특수한 키워드

- super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출
- super를 참조하면 수퍼클래스의 메서드를 호출 가능

### 25.8.6 상속 클래스의 인스턴스 생성 과정

1. 서브클래스의 super 호출
2. 수퍼클래스의 인스턴스 생성과 this 바인딩
3. 수퍼클래스의 인스턴스 초기화
4. 서브클래스 constructor로의 복귀와 this 바인딩
5. 서브클래스의 인스턴스 초기화
6. 인스턴스 반환

### 25.8.7 표준 빌트인 생성자 함수 확장

extends 키워드 다음에는 클래스뿐만이 아니라 \[\[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용 가능  
String, Number, Array 같은 표준 빌트인 객체도 extends 키워드를 사용하여 확장 가능

```js
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 중복된 배열 요소를 제거하고 반환: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구함: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75
```

Array 생성자 함수를 상속받아 확장한 MyArray 클래스가 생성한 인스턴스는 Array.prototype과 MyArray.prototype의 모든 메서드 사용 가능
