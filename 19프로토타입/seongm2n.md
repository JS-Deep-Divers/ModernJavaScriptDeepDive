### 19장

- 자바스크립트는 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강략한 객체지향 프로그래밍 능력을 지니고 있는

프로토타입 기반의 객체지향 프로그램이 언어!

- 자바스트립트를 이루고 있는 거의 “모든 것”이 객체
- 원시 타입의 값을 제외한 나머지 값들(함수,배열,정규 표현식 등)은 모두 객체

[19.6] 객체 생성 방식과 프로토타입의 결정

- 객체 생성 방법
    1. 객체 리터럴
    2. Object 생성자 함수
    3. 생성자 함수
    4. Object.create 메서드
    5. 클래스(ES6)

    ⇒ 공통점 :  추상 연산에 의해 생성


[19.7] 프로토타입 체인

```jsx
function Person(name){
	this.name= name;
}
Person.prototype.sayHello = function (){
	console.log(`Hi! My name is ${this.name}`};
};
const me = new Person('Lee');
//hasOwnProperty는 Object.prototype의 메서드
console.log(me.hasOwnProperty('name'));
```

- 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색
- 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 매커니즘

    ❗프로토타입 체인 : 상속과 프로퍼티 검색을 위한 메커니즘

    ❗스코프 체인 : 식별자 검색을 위한 매커니즘

    ⇒ 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용


[19.8] 오버라이딩과 프로퍼티 섀도잉

❗오버라이딩

→ 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

❗오버로딩

→ 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식

→ 자바스크립트는 오버로딩을 지원하지 않지만 arguments객체를 사용하여 구현은 가능

[19.10] instanceof 연산자

```
객체 instanceof 생성자 함수
```

⇒ 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 프로토타입 체인 상에 존재하면 true로 평가, 그렇지 않은 경우 false로 평가

[19.13] 프로퍼티 존재 확인

(1) in 연산자

- 객체 내에 특정 프로퍼티가 존재하는지 여부 확인
- in 대신 Reflect.has메서드 사용

```jsx
/**
*key: 프로퍼티 키를 나타내는 문자열
*object: 객체로 평가되는 표현식
*/
key in object
```

```jsx
// in 연산자
const person = {
	name: 'Lee',
	address:'Seoul'
};
console.log('name' in person) //true
console.log('address' in person) //true
console.log('age' in person) //false

//Reflect.has메서드 사용
const person = {name: 'Lee'};
console.log(Reflect.has(person, 'name'); //true
console.log(Reflect.has(person, 'toString'); //true
```

(2) Object.prototype.hasOwnProperty메서드

- 프로퍼티의 존재 여부 확인
- 고유의 프로퍼티 키인 경우에만 true반환, 상속받은 프로토타입의 프로퍼티 키인 경우 false반환

```jsx
//Reflect.has메서드 사용
const person = {name: 'Lee'};
console.log(person.hasOwnProperty('name'); //true
console.log(person.hasOwnProperty('age'); //false
console.log(person.hasOwnProperty('toString'); //false
```

[19.14] 프로퍼티 열거

(1) for…in문

- 객체의 모든 프로퍼티를 순회하며 열거 하려면 for…in문을 사용
- 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트의 값이 true인 프로퍼티를 순회하며 열거
```
for(변수선언문 in 객체){...}
```
