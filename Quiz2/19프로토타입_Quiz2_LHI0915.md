# Quiz2

<br>

## 19. 프로토타입

### 1. 모든 객체가 자신의 프로토타입에 접근하기 위해서는 a 접근자 프로퍼티를 이용합니다. 즉, b 내부 슬롯에 간접적으로 접근하게 됩니다. 여기서 a와 b에 들어갈 것은 무엇일까요?

<br>

### 2. 프로토타입의 생성 시점은 언제인가요?

### 3. console.log의 결과를 작성해주세요.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(`Hi! My Name is ${this.name}`);
};

const me = new Person('Lee');

console.log(me.hasOwnProperty('name'));
console.log(Object.getPrototypeOf(me) === Person.prototype);
console.log(Object.getPrototypeOf(Person.prototype) === Object.prototype);
```
