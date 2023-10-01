# 45장. 프로미스

자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용

전통적인 콜백 패턴의 단점

- 콜백 헬로 인해 가독성 &darr;
- 비동기 처리 중 발생한 에러의 처리가 곤란
- 여러 개의 비동기 처리를 한 번에 처리하는 데 한계 존재

ES6에서 비동기처리를 위한 패턴 프로미스 도입  
콜백 패턴이 가진 단점 보완 및 비동기 처리 시점 명확하게 표현 가능

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

### 45.1.1 콜백 헬

비동기 함수: 함수 내부에 비동기로 동작하는 코드를 포함한 함수

비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료  
비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료  
비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없음
따라서 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행 필요

비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적  
필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있음  
콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩, 복잡도가 높아지는 현상이 발생(콜백 헬)  
콜백 헬은 가독성 저해 및 실수 유발의 원인

```js
// 콜백 헬이 발생하는 전형적인 사례
get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

### 45.1.2 에러 처리의 한계

비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 것

```js
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  // 에러를 캐치하지 못함
  console.error("캐치한 에러", e);
}
```

try 코드 블록 내에서 호출한 setTimeout 함수는 1초 후에 콜백 함수가 실행되도록 타이머 설정, 이후 콜백 함수는 에러 발생  
그러나 에러는 catch 코드 블록에서 캐치되지 못함  
이유 &rarr;  
비동기 함수인 setTimeout이 호출되면 setTimeout 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸시되어 실행  
setTimeout은 비동기 함수이므로 즉시 종료되어 콜 스택에서 제거  
타이머 만료되면 setTimeout 함수의 콜백 함수는 태스크 큐로 푸시, 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행  
setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태(setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아님)  
에러는 호출자 방향으로, 즉 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파  
setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니기 때문에 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않음

## 45.2 프로미스의 생성

ES6 도입  
Promise 생성자 함수를 new 연산자와 함께 호출하면 Promise 객체를 생성  
Promise는 호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체

Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받음  
이 콜백 함수는 resolve와 reject 함수를 인수로 전달받음

```js
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행
  if (/* 비동기 처리 성공 */) {
    resolve('result');
  } else { /* 비동기 처리 성공 */
    reject('failure reason');
  }
});
```

Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행

- 비동기 처리 성공: 콜백 함수의 인수로 전달받은 resolve 함수를 호출
- 비동기 처리 실패: 콜백 함수의 인수로 전달받은 reject 함수를 호출

프로미스가 가지고 있는 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보
|프로미스의 상태 정보|의미|상태 변경 조건|
|:---|:---|:---|
|pending|비동기 처리가 아직 수행되지 않은 상태|프로미스가 생성된 직후 기본 상태|
|**fulfilled**|비동기 처리가 수행된 상태(성공)|resolve 함수 호출|
|**rejected**|비동기 처리가 수행된 상태(실패)|reject 함수 호출|

생성된 직후의 프로미스는 기본적으로 pending 상태  
이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 프로미스의 상태가 변경

- 비동기 처리 성공: resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
- 비동기 처리 실패: reject 함수를 호출해 프로미스를 rejected 상태로 변경

프로미스의 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정됨  
프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체

## 45.3 프로미스의 후속 처리 메서드

프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리 필요  
이를 위해 프로미스는 후속 메서드 then, catch, finally를 제공

프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출  
후속 처리 메서드의 콜백 함수에 프로미스의 처리 결과가 인수로 전달  
모든 후속 처리 메서드는 프로미스를 반환, 비동기로 동작

프로미스의 후속 처리 메서드

1. Promise.prototype.then
2. Promise.prototype.catch
3. Promise.prototype.finally

### 45.3.1 Promise.prototype.then

then 메서드는 두 개의 콜백 함수를 인수로 전달받음

- 첫 번째 콜백 함수  
  프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출  
  이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달 받음
- 두 번째 콜백 함수  
  프로미스가 rejected 상태(reject 함수가 호출된 상태)가 되면 호출  
  이때 콜백 함수는 프로미스의 에러를 인수로 전달받음

```js
//fulfilled
new Promise((resolve) => resolve("fulfilled")).then(
  (v) => console.log(v),
  (e) => console.error(e)
); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(
  (v) => console.log(v),
  (e) => console.error(e)
); // Error: rejected
```

then 메서드는 언제나 프로미스를 반환  
then 메서드의 콜백 함수  
 프로미스를 반환 &rarr; 프로미스를 그대로 반환
프로미스가 아닌 값을 반환 &rarr; 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환

### 45.3.2 Promise.prototype.catch

catch 메서드는 한 개의 콜백 함수를 인수로 전달받음  
catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우만 호출

```js
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) =>
  console.log(e)
); // Error: rejected
```

catch 메서드는 then(undefined, onRejected)과 동일하게 동작  
따라서 then 메서드와 마찬가지로 언제나 프로미스를 반환

```js
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) =>
  console.log(e)
); // Error: rejected
```

### 45.3.3 Promise.prototype.finally

finally 메서드는 한 개의 콜백 함수를 인수로 전달받음  
finally 메서드의 콜백 함수는 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한 번 호출, 언제나 프로미스 반환  
finally 메서드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용

```js
new Promise(() => reject(new Error("rejected"))).then(undefined, (e) =>
  console.log(e)
); // Error: rejected
```

## 45.4 프로미스의 에러 처리

비동기 처리를 위한 콜백 패턴은 에러 처리가 곤란하다는 문제 존재  
프로미스는 에러를 문제없이 처리 가능

비동기 처리 결과에 대한 후속 처리는 프로미스가 제공하는 후속 처리 메서드 then, catch, finally를 사용하여 수행

비동기 처리에서 발생한 에러 처리

- then 메서드의 두 번째 콜백 함수로 처리

  ```js
  const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

  // 부적절한 URL이 지정되었기 때문에 에러 발생
  promiseGet(wrongUrl).then(
    (res) => console.log(res),
    (err) => console.error(err)
  ); // Error: 404
  ```

- 프로미스의 후속 처리 메서드 catch 사용

  ```js
  const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

  // 부적절한 URL이 지정되었기 때문에 에러 발생
  promiseGet(wrongUrl)
    .then((res) => console.log(res))
    .catch((err) => console.error(err)); // Error: 404
  ```

에러 처리시 then 메서드보다 catch 메서드를 권장하는 이유

- catch 메서드를 모든 then 메서드를 호출한 이후에 호출  
  &rarr; 비동기 처리에서 발생한 에러뿐만이 아닌 then 메서드 내부에서 발생한 에러까지 모두 캐치
- then 메서드에 두 번째 콜백 함수를 전달하는 것보다 catch 메서드를 사용하는 것이 가독성이 좋고 명확

## 45.5 프로미스 체이닝

비동기 처리를 위한 콜백 패턴은 콜백 헬이 발생하는 문제 존재  
프로미스는 then, catch, finally 후속 처리 메서드를 통해 콜백 헬을 해결

```js
const url = "https://jsonplaceholder.typicode.com";

// id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
  // 취득한 post의 userId로 user 정보를 취득
  .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
  .then((userInfo) => console.log(userInfo))
  .catch((err) => console.error(err));
```

then &rarr; then &rarr; catch 순서로 후속 처리 메서드 호출  
then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출 가능  
이를 **프로미스 체이닝**이라 함

프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬이 발생하지 않음(프로미스도 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아님)

콜백 패턴의 가독성이 좋지 않은 문제는 ES8에서 도입된 async/await를 통해 해결 가능  
async/await를 사용하면 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능

```js
const url = "https://jsonplaceholder.typicode.com";

(async () => {
  // id가 1인 post의 userId를 취득
  const { userId } = await promiseGet(`${url}/posts/1`);

  // 취득한 post의 userId로 user 정보를 취득
  const userInfo = await promiseGet(`${url}/users/${userId}`);

  console.log(userInfo);
})();
```

## 45.6 프로미스의 정적 메서드

Promise는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있음  
Promise는 5가지 정적 메서드를 제공

### 45.6.1 Promise.resolve / Promise.reject

Promise.resolve와 Promise.reject 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용

- Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성
  ```js
  // 배열을 resolve하는 프로미스를 생성
  const resolvedPromise = Promise.resolve([1, 2, 3]);
  resolvedPromise.then(console.log); // [1, 2, 3]
  ```
  위 예제와 아래 예제는 동일하게 동작
  ```js
  const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));
  resolvedPromise.then(console.log); // [1, 2, 3]
  ```
- Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성
  ```js
  // 에러 객체를 reject하는 프로미스를 생성
  const rejectedPromise = Promise.reject(new Error("Error!"));
  rejectedPromise.catch(console.log); // Error: Error!
  ```
  위 예제와 아래 예제는 동일하게 동작
  ```js
  const rejectedPromise = new Promise((_, reject) =>
    reject(new Error("Error!"))
  );
  rejectedPromise.catch(console.log); // Error: Error!
  ```

### 45.6.2 Promise.all

Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용

```js
const requestData1 = () =>
  new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () =>
  new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () =>
  new Promise((resolve) => setTimeout(() => resolve(3), 1000));

// 세 개의 비동기 처리를 병렬로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
  .then(console.log) // [ 1, 2, 3 ] ⇒ 약 3초 소요
  .catch(console.error);
```

Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달 받음  
그리고 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환  
Promise.all 메서드는 처리 순서 보장, 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료

### 45.6.3 Promise.race

Promise.race 메서드는 Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음  
Promise.race 메서드는 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환

```js
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

Promise.race 메서드에 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스 즉시 반환

### 45.6.4 Promise.allSettled

ES11 도입  
Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음  
전달받은 프로미스가 모두 settled 상태가 되면 처리 결과를 배열로 반환

```js
Promise.allSettled([
  new Promise((resolve) => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Error!")), 1000)
  ),
]).then(console.log);
/*
[
  {status: "fulfilled", value: 1},
  {status: "rejected", reason: Error: Error! at <anonymous>:3:54}
]
*/
```

Promise.allSettled 메서드가 반환한 배열에는 fulfilled 또는 rejected 상태와는 상관없이 Prmoise.allSettled 메서드가 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨 있음

프로미스의 처리 결과를 나타내는 객체

- 프로미스가 fulfilled 상태  
  &rarr; 비동기 처리 상태를 나타내는 status 프로퍼티, 처리 결과를 나타내는 value 프로퍼티
- 프로미스가 rejected 상태  
  &rarr; 비동기 처리 상태를 나타내는 status 프로퍼티, 에러를 나타내는 reason 프로퍼티

## 45.7 마이크로태스크 큐

다음 예제 코드의 로그 출력 순서는?

```js
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
```

프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 &rarr; 2 &rarr; 3 출력될 것처럼 보이지만, 2 &rarr; 3 &rarr; 1 출력  
프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장되기 때문

마이크로태스크 큐는 태스크 큐와는 별도의 큐

- 마이크로태스크 큐: 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장
- 태스크 큐: 그 외의 비동기 함수, 콜백 함수, 이벤트 핸들러 일시 저장

**마이크로태스크 큐는 태스트큐보다 우선순위가 높음**  
이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행  
이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행

## 45.8 fetch

fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API  
fetch 함수는 XMLHttpRequest 객체보다 사용법이 간단하고, 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유로움

fetch 함수에는 HTTP 요청을 전송할 URL, HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달

`const promise = fetch(url [, options])`

fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환  
따라서 후속처리 메서드 then을 통해 프로미스가 resolve한 Response 객체를 전달받을 수 있음  
Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공

예) fetch 함수가 반환한 프로미스가 래핑하고 있는 MIME 타입이 application/json인 HTTP 응답 몸체를 취득  
&rarr; Response.prototype.json 메서드 사용(Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화)

```js
fetch("https://jsonplaceholder.typicode.com/todos/1")
  // response는 HTTP 응답을 나타내는 Response 객체
  // json 메서드를 사용, Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화
  .then((response) => response.json())
  // json은 역직렬화된 HTTP 응답 몸체
  .then((json) => console.log(json));
// {userId: 1, id: 1, title: "delectus aut autem", completed: false}
```

fetch 함수를 사용할 때 에러 처리에 주의  
fetch 함수가 반환하는 프로미스는 HTTP 에러 발생해도 에러를 reject하지 않고 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 resolve  
오프라인 등의 네트워크 장애, CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스 reject

따라서 fetch 함수가 반환한 프로미스가 resolve한 불리언 타입의 ok 상태를 확인해 명시적으로 에러 처리

```js
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL이 지정되었기 때문에 404 Not Found 에러가 발생
fetch(wrongUrl)
  // response는 HTTP 응답을 나타내는 Response 객체
  .then((response) => {
    if (!response.ok) throw new Error(response.statusText);
    return response.json();
  })
  .then((todo) => console.log(todo))
  .catch((err) => console.error(err));
```
