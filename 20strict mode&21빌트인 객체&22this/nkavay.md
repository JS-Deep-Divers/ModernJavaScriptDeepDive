
- 화살표 함수 this

상위 스코프의 this

```javascript
const value = 1;

const obj = {
  value: 100,
  arrowFn() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100);
  }
};

//debugger;

obj.arrowFn();
```

## callback 함수 this

전역 객체

```javascript
const value = 1;

const obj = {
  value: 100,
  fn: function() {
    setTimeout(function() {
      console.log("callback's this: ",  this);
      console.log("callback's this.value: ",  this.value);
      //debugger;
    }, 100);
  }
};

obj.fn();
```

## this binding

bind, apply, call

```javascript
const displayThis = function (key, value) {
  console.dir(this);
  console.log("key",key);
  console.log("value", value);
};

//debugger;

displayThis();

const obj = { displayThis };
obj.displayThis();

const bindingObject = { name: 'binding' };
displayThis.call(bindingObject, 'language', 'javascript');
displayThis.apply(bindingObject, ['language', 'typescript']);
displayThis.bind(bindingObject)();
```

- 생성자 함수 this

- new 키워드 없을 경우 일반 함수처럼 동작하고 리턴이 없을 경우 undefined반환
- new 키워드 있을 경우 this를 해당 object로 binding해주고 리턴을 명시하지 않아도 암묵적으로 (해당 object) this를 반환

```javascript
//debugger

const me = Person('Lee', 23);

console.log(me);
console.log({name: window.name, age: window.age});

const itsMe = new Person('Kim', 19);

console.log(itsMe);
console.log({name: window.name, age: window.age});
console.log({name: itsMe.name, age: itsMe.age});

function Person(name, age) {
  this.name = name;
  this.age = age;
};
```

