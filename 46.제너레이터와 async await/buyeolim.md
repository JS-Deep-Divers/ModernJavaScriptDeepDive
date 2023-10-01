# 46장. 제너레이터와 async/await

## 46.1 제너레이터란?

ES6 도입  
코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

제너레이터와 일반 함수의 차이

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있음
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있음
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환

## 46.2 제너레이터 함수의 정의

제너레이터 함수는 function\* 키워드로 선언  
그리고 하나 이상의 yield 표현식 포함  
나머지는 일반 함수 정의와 동일

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethod() {
    yield 1;
  }
}
```

애스터리스크(\*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 유효하나 일관성 유지를 위해 function 키워드 바로 뒤에 붙이는 것을 권장

!주의
제너레이터 함수는 화살표 함수로 정의 불가  
제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출 불가

## 46.3 제너레이터 객체

제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환  
제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터

제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에 없는 return, throw 메서드를 보유  
제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작

- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환
- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환

```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End!")); // {value: "End!", done: true}
```

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환

```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.throw("Error!")); // {value: undefined, done: true}
```

## 46.4 제너레이터의 일시 중지와 재개

제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개 가능

일반 함수 &rarr; 호출 이후 제어권을 함수가 독점  
제너레이터 &rarr; 함수 호출자에게 제어권을 양도하여 필요한 시점에 함수 실행 재개 가능

제너레이터 함수 호출  
&rarr; 제너레이터 객체 반환  
&rarr; 제너레이터 객체는 next 메서드 보유  
&rarr; 제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수의 코드 블록 실행

일반 함수 &rarr; 한 번에 코드 블록의 모든 코드를 일괄 실행  
제너레이터 &rarr; yield 표현식까지만 실행

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고 받을 수 있음  
제너레이터의 특성을 활용하면 비동기 처리를 동기 처리처럼 구현 가능

## 46.5 제너레이터의 활용

### 46.5.1 이터러블의 구현

제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현 가능

### 46.5.2 비동기 처리

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있음  
이 특성을 활용해 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현 가능

## 46.6 async/await

ES8 도입  
비동기 처리를 동기 처리처럼 동작하도록 구현하는데 제너레이터보다 간단, 가독성 &uarr;

async/await는 프로미스를 기반으로 동작  
이를 사용해 프로미스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달, 비동기 처리 결과를 후속 처리할 필요 없이 동기 처리처럼 프로미스 사용 가능

### 46.1 async 함수

await 키워드는 반드시 async 함수 내부에서 사용  
async 함수는 async 키워드를 사용해 정의, 언제나 프로미스 반환  
async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환

```js
// async 함수 선언문
async function foo(n) {
  return n;
}
foo(1).then((v) => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) {
    return n;
  },
};
obj.foo(4).then((v) => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) {
    return n;
  }
}
const myClass = new MyClass();
myClass.bar(5).then((v) => console.log(v)); // 5
```

### 46.2 await 키워드

await 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환  
await 키워드는 반드시 프로미스 앞에서 사용해야 함

await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개

```js
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요
```

모든 프로미스에 await 키워드를 사용하는 것은 주의  
위 예제는 foo 함수 종료까지 총 6초 소요  
&rarr; 첫 번째 프로미스 settled 상태까지 3초 소요  
&rarr; 첫 번째 프로미스 settled 상태까지 2초 소요  
&rarr; 첫 번째 프로미스 settled 상태까지 1초 소요

서로의 비동기 함수가 연관이 없어 순서가 보장되지 않아도 된다면 비동기 처리가 완료될 때까지 대기해서 순차적으로 처리할 필요 없음

```js
async function foo() {
  const res = await Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
  ]);

  console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요
```

### 46.3 에러 처리

비동기 처리를 위한 콜백 패턴의 단점 중 가장 심각한 것 &rarr; 에러 처리 곤란  
에러는 호출자 방향, 즉 콜스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파  
비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try ..catch 문으로 에러 캐치 불가

```js
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  // 에러를 캐치 못함
  console.error("캐치한 에러", e);
}
```

async/await에서 에러 처리는 try ... catch 문 사용 가능

```js
const foo = async () => {
  try {
    const wrongUrl = "https://wrong.url";

    const response = await fetch(wrongUrl);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err); // TypeError: Failed to fetch
  }
};

foo();
```

async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스 반환  
따라서 async 함수를 호출, Promise.prototype.catch 후속 처리 메서드를 사용해 에러 캐치 가능

```js
const foo = async () => {
  const wrongUrl = "https://wrong.url";

  const response = await fetch(wrongUrl);
  const data = await response.json();
  return data;
};

foo().then(console.log).catch(console.error); // TypeError: Failed to fetch
```
