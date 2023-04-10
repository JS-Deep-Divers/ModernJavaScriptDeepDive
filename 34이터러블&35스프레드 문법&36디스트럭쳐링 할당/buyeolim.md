# 34장. 이터러블

## 34.1 이터레이션 프로토콜

ES6 도입  
이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)를 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙
ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for.. of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화

이터레이션 프로토콜

- 이터러블 프로토콜
- 이터레이터 프로토콜

### 34.1.1 이터러블

이터러블 프로토콜을 준수한 객체  
Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체

- Symbol.iterator 메서드를 상속받는 이터러블은 for ... of 문으로 순회, 스트레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능

  ```js
  const array = [1, 2, 3];

  // 배열은 Array.prototype의 Symbol.iterator
  console.log(Symbol.iterator in array) // true

  // 이터러블인 배열은 for ... of 문으로 순회 가능
  for (const item of array) {
    console.log(item);
  }

  // 이터러블인 배열은 스프레드 문법의 대상으로 사용 가능
  console.log([...array]); // [1, 2, 3]

  // 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용
  const [a, ...rest] = array;
  console.log(a, rest)); // 1, [2, 3]
  ```

- Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니기 때문에 for .. of 문 순회, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 불가

  ```js
  const obj = { a: 1, b: 2 };

  // 일반 객체는 Symbol.iterator 메서드를 구현, 상속받지 않음
  // 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아님
  console.log(Symbol.iterator in obj); // false

  // 이터러블이 아닌 일반 객체는 for ... of 문으로 순회 불가
  for (const item of obj) {
    // -> TypeError: obj is not iterable
    console.log(item);
  }

  // 이터러블이 아닌 일반 객체는 배열 디스트럭처링 할당의 대상으로 사용 불가
  const [a, b] = obj; // -> TypeError: obj is not iterable
  ```

  단, 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용

  ```js
  const obj = { a: 1, b: 2 };

  // 스프레드 프로퍼티 제안(stage 4)은 객체 리터럴 내부에서 스프레드 문법의 사용 하용
  console.log({ ...obj }); // { a: 1, b: 2 }
  ```

### 34.1.2 이터레이터

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환  
이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 보유  
이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할  
next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환

```js
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환, 이터레이터는 next 메서드 보유
const iterator = array[Symbol.iterator]();

// next 메서드 호출 -> 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체 반환
// 이터레이터 리절트 객체는 value, done 프로퍼티를 갖는 객체
// value 프로퍼티: 현재 순회 중인 이터러블의 값
// done 프로퍼티: 이터러블의 순회 완료 여부
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 34.2 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블 제공

| 빌트인 이터러블 | Symbol.iterator 메서드                                                             |
| :-------------- | :--------------------------------------------------------------------------------- |
| Array           | Array.prototype\[Symbol.iterator]                                                  |
| String          | String.prototype\[Symbol.iterator]                                                 |
| Map             | Map.prototype\[Symbol.iterator]                                                    |
| Set             | Set.prototype\[Symbol.iterator]                                                    |
| TypeArray       | TypeArray.prototype\[Symbol.iterator]                                              |
| arguments       | arguments.prototype\[Symbol.iterator]                                              |
| DOM 컬렉션      | NodeList.prototype\[Symbol.iterator]<br>HTMLCollection.prototype\[Symbol.iterator] |

## 34.3 for ... of 문

for .. of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당  
`for (변수선언문 of 이터러블) { ... }`

for ... of 문은 for ... in 문의 형식과 매우 유사

- for ... in 문  
  객체의 프로토 타입 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 \[\[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거  
  프로퍼티 키가 심벌인 프로퍼티는 열거하지 않음
- for .. of 문  
  내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for ... of 문의 변수에 할당  
  이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단
  ```js
  for (const item of [1, 2, 3]) {
    console.log(item); // 1 2 3
  }
  ```

## 34.4 이터러블과 유사 배열 객체

유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체

- length 프로퍼티  
  &rarr; for 문으로 순회 가능
- 인덱스를 나타내는 숫자 형식의 문자열의 프로퍼티 키  
  &rarr; 배열처럼 인덱스로 프로퍼티 값에 접근 가능

```js
// 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

for (let i = 0; i < arrayLike.length; i++) {
  console.log(arrayLike[i]); // 1 2 3
}
```

유사 배열 객체는 이터러블이 아닌 일반 객체, Symbol.iterator 메서드가 없기 때문에 for ... of 문으로 순회 불가  
단, arguments, NodeList, HTMLCollection, 배열은 유사 배열 객체이면서 이터러블  
모든 유사 배열 객체가 이터러블이 아닌 것에 주의

## 34.5 이터레이션 프로토콜의 필요성

- ES6 이전  
  순회 가능한 데이터 컬렉션(배열, 문자열, 유사 배열 객체, DOM 컬렉션 등)은 통일된 규약 없이 각자 나름의 구조를 가지고 for 문, for ... in 문, forEach 메서드 등 다양한 방법으로 순회 가능
- ES6  
  순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for .. of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화

이터러블은 for ... of 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자의 역할  
이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정  
데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할

## 34.6 사용자 정의 이터러블

### 34.6.1 사용자 정의 이터러블 구현

이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블

이터레이션 프로토콜 준수사항

- Symbol.iterator 메서드 구현
- Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터 반환
- 이터레이터의 next 메서드는 done, value 프로퍼티를 가지는 이터레이터 리절트 객체 반환
- for ... of 문은 done 프로퍼티가 true가 될 때까지 반복, done 프로퍼티가 true가 되면 반복 중지

이터러블은 for ... of 문뿐만 아니라 스프레드 문법, 배열 디스트럭처링 할당에도 사용 가능

### 34.6.2 이터러블을 생성하는 함수

### 34.6.3 이터러블이면서 이터레이터인 객체를 생성하는 함수

### 34.6.4 무한 이터러블과 지연 평가

**지연 평가**  
데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 비로소 데이터를 생성하는 기법  
평가 결과가 필요할 때까지 평가를 늦추는 기법
빠른 실행 속도 기대, 불필요한 메모리 소비하지 않으며 무한도 표현 가능

# 35장. 스프레드 문법

ES6 도입  
스프레드 문법(전개 문법) ... 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 생성  
사용할 수 있는 대상은 for ... of 문으로 순회할 수 있는 이터러블에 한정

```js
console.log(...[1, 2, 3]); // 1 2 3
console.log(..."Hello"); // H e l l o
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // ['a', '1'] ['b', '2']
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없음
console.log(...{ a: 1, b: 2 });
// TypeError: Spread syntax requires ...iterable[Symbol.iterator] to be a function
```

스프레드 문법의 결과는 값이 아님  
스프레드 문법 ... 이 피연산자를 연산하여 값을 생성하는 연산자가 아님  
따라서 스프레드 문법의 결과는 변수에 할당 불가

스프레드 문법의 결과물은 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용 가능

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

요소들의 집합인 배열을 펼쳐, 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야하는 경우

```js
const arr = [1, 2, 3];

Math.max(arr); // -> NaN
Math.max(...arr); // -> 3
```

스프레드 문법은 Rest 파라미터와 형태가 동일하여 혼동할 수 있으므로 주의

## 35.2 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존의 방식보다 더욱 간결하고 가독성 좋게 표현 가능

### 35.2.1 concat

2개의 배열을 1개의 배열로 결합하고 싶은 경우

- ES5: concat 메서드 사용
  ```js
  var arr = [1, 2].concat([3, 4]);
  console.log(arr); // [1, 2, 3, 4]
  ```
- ES6: 스프레드 문법 사용
  ```js
  const arr = [...[1, 2], ...[3, 4]];
  console.log(arr); // [1, 2, 3, 4]
  ```

### 35.2.2 splice

배열의 중간에 다른 배열의 요소들을 추가, 제거

- ES5: splice 메서드 및 Function.prototype.apply 메서드 사용

  ```js
  var arr1 = [1, 4];
  var arr2 = [2, 3];

  // arr2를 해체하여 전달 필요
  Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
  console.log(arr1); // [1, 2, 3, 4]
  ```

- ES6: 스프레드 문법 사용

  ```js
  const arr1 = [1, 4];
  const arr2 = [2, 3];

  arr1.splice(1, 0, ...arr2);
  console.log(arr1); // [1, 2, 3, 4]
  ```

### 35.2.3 배열 복사

- ES5: slice 메서드 사용

  ```js
  var origin = [1, 2];
  var copy = origin.slice(); // 얕은 복사

  console.log(copy); // [1, 2]
  console.log(copy === origin); // false
  ```

- ES6: 스프레드 문법 사용

  ```js
  const origin = [1, 2];
  const copy = [...origin]; // 얕은 복사

  console.log(copy); // [1, 2]
  console.log(copy === origin); // false
  ```

### 35.2.4 이터러블을 배열로 변환

- ES5: Function.prototype.apply 또는 Function.prototype.call 메서드를 사용하여 slice 메서드 호출

  ```js
  function sum() {
    // 이터러블이면서 유사 배열 객체인 arguments를 배열로 반환
    var args = Array.prototype.slice.call(arguments);

    return args.reduce(function (pre, cur) {
      return pre + cur;
    }, 0);
  }

  console.log(sum(1, 2, 3)); // 6
  ```

  유사 배열 객체도 이를 이용해 배열로 변환 가능

- ES6: 스프레드 문법 또는 Rest 파라미터 사용

  ```js
  // 스프레드 문법
  function sum() {
    // 이터러블이면서 유사 배열 객체인 arguments를 배열로 반환
    return [...arguments].reduce((pre, cur) => pre + cur, 0);
  }

  console.log(sum(1, 2, 3)); // 6
  ```

  ```js
  // Rest 파라미터
  const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

  console.log(sum(1, 2, 3)); // 6
  ```

  단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없음  
  이는 ES6에서 도입된 Array.from 메서드 사용

  ```js
  const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3,
  };

  Array.from(arrayLike); // -> [1, 2, 3]
  ```

## 35.3 객체 리터럴 내부에서 사용하는 경우

Rest 프로퍼티와 함께 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법 사용 가능  
스프레드 문법의 대상은 이터러블이어야하지만 스프레드 프로퍼티 제안[🔗](https://github.com/tc39/proposal-object-rest-spread)은 일반 객체를 대상으로 스프레드 문법의 사용을 허용

```js
// 스프레드 프로퍼티
// 객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 Object.assign 메서드를 사용하여 어러 개의 객체르 ㄹ병합, 특정 프로퍼티를 변경 또는 추가  
스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법

# 36장. 디스트럭처링 할당

디스트럭처링 할당(구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당  
배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용

## 36.1 배열 디스트럭처링 할당

- ES5

  ```js
  var arr = [1, 2, 3];

  var one = arr[0];
  var two = arr[1];
  var three = arr[2];

  console.log(one, two, three); // 1 2 3
  ```

- ES6

  ```js
  const arr = [1, 2, 3];

  const [one, two, three] = arr; // 할당 기준은 배열의 인덱스

  console.log(one, two, three); // 1 2 3
  ```

  변수에 기본값 설정 가능(할당값 우선)

  ```js
  const [a, b = 10, c = 3] = [1, 2];
  console.log(a, b, c); // 1 2 3
  ```

  변수에 Rest 파라미터와 유사하게 **Rest 요소** ... 사용 가능(반드시 마지막에 위치)

  ```js
  const [x, ...y] = [1, 2, 3];
  console.log(x, y); // 1, [ 2, 3 ]
  ```

## 36.2 객체 디스트럭처링 할당

- ES5

  ```js
  var user = { firstName: "Ungmo", lastName: "Lee" };

  var firstName = user.firstName;
  var lastName = user.lastName;

  console.log(firstName, lastName); // Ungmo Lee
  ```

- ES6

  ```js
  const user = { firstName: "Ungmo", lastName: "Lee" };

  const { lastName, firstName } = user;

  console.log(firstName, lastName); // Ungmo Lee
  ```

  객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값 할당

  ```js
  const user = { firstName: "Ungmo", lastName: "Lee" };

  const { lastName: ln, firstName: fn } = user;

  console.log(fn, ln); // Ungmo Lee
  ```

  변수에 기본값 설정

  ```js
  const { firstName = "Ungmo", lastName } = { lastName: "Lee" };
  console.log(firstName, lastName); // Ungmo Lee
  ```

  객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당

  ```js
  const str = "Hello";

  const { length } = str; // String 래퍼 객체로부터 length 프로퍼티만 추출
  console.log(length); // 5

  const todo = { id: 1, content: "HTML", completed: true };

  const { id } = todo; // todo 객체로부터 id 프로퍼티만 추출
  console.log(id); // 1
  ```

  객체를 인수로 전달받는 함수의 매개변수에도 사용 가능

  ```js
  function printTodo({ content, completed }) {
    console.log(
      `할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`
    );
  }

  printTodo({ id: 1, content: "HTML", completed: true }); // 할일 HTML은 완료 상태입니다.
  ```

  변수에 Rest 파라미터나 Rest 요소와 유사하게 **Rest 프로퍼티** ... 사용 가능(반드시 마지막에 위치)

  ```js
  const { x, ...rest } = { x: 1, y: 2, z: 3 };
  console.log(x, rest); // 1, { y: 2, z: 3 }
  ```
