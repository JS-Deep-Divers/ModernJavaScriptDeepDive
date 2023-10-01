# 41장. 타이머

## 41.1 호출 스케줄링

호출 스케줄링: 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하기 위해 타이머 함수 사용

자바스크립트가 제공하는 타이머 함수

- 타이머 생성: setTimeout, setInterval
- 타이머 제거: clearTimeout, clearInterval

타이머 함수는 ECMAScript 사양에 정의된 빌트인 함수가 아님  
브라우저 환경, Node.js 환경에서 모두 전역 객체의 메서드로서 타이머 함수를 제공  
타이머 함수는 호스트 객체

자바스크립트 엔진은 싱글 스레드로 동작  
타이머 함수 setTimeout과 setInterval은 비동기 처리 방식으로 동작

## 42.2 타이머 함수

### 42.2.1 setTimeout / clearTimeout

setTimeout 함수는 두 번째 인수로 전달받은 시간(ms)으로 단 한 번 동작하는 타이머를 생성  
타이머 만료 후, 첫 번째 인수로 전달받은 콜백 함수가 호출  
즉, setTimeout 함수의 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케줄링  
setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환

```js
// 1초 (1000ms) 후 타이머가 만료되면 콜백 함수가 호출
setTimeout(() => console.log("Hi!"), 1000);
```

clearTimeout 함수는 호출 스케줄링을 취소

```js
// 1초 (1000ms) 후 타이머가 만료되면 콜백 함수가 호출
// setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환
const timerId = setTimeout(() => console.log("Hi!"), 1000);

// setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머 취소
// 타이머가 취소되면 setTimeout 함수의 콜백 함수가 실행되지 않음
clearTimeout(timerId);
```

### 42.2.2 setInterval / clearInterval

setInterval 함수는 두 번째 인수로 전달받은 시간(ms)으로 반복 동작하는 타이머를 생성  
이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출  
이는 타이머가 취소될 때까지 계속  
즉, setInterval 함수의 콜백 함수는 두 번째 인수로 전달받은 시간이 경과할 때마다 반복실행되도록 호출 스케줄링  
setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환

clearInterval 함수는 호출 스케줄링 취소

```js
let count = 1;

// 1초(1000ms) 후 타이머가 만료될 때마다 콜백 함수가 호출
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환
const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5

  // count가 5이면 setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소
  // 타이머가 취소되면 setInterval 함수의 콜백 함수가 실행되지 않음
  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 41.3 디바운스와 스로틀

scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생  
이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능 문제 발생 가능성 &uarr;  
디바운스와 스로틀: 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법

### 41.3.1 디바운스

디바운스: 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 수행

### 41.3.2 스로틀

스로틀: 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 생성

# 43장. Ajax

## 43.1 Ajax란?

Ajax(Asynchronous JavaScript and XML): 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청, 서버가 응답한 데이터를 수신, 웹페이지를 동적으로 갱신하는 프로그래밍 방식

Ajax는 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작  
XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공

이전의 웹페이지는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작  
화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링

전통적인 방식의 단점

1. 변경할 필요가 없는 부분까지 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생
2. 변경할 필요가 없는 부분까지 처음부터 다시 렌더링, 이로 인한 화면 전환 시 순간적으로 깜박이는 현상 발생
3. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을때까지 다음 처리는 블로킹

Ajax 방식의 장점

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
2. 변경할 필요가 없는 부분은 다시 렌더링하지 않아 화면이 순간적으로 깜박이는 현상 발생하지 않음
3. 클라이언트, 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않음

## 43.2 JSON

JSON은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷  
자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷, 대부분의 프로그래밍 언어에서 사용 가능

### 43.2.1 JSON 표기 방식

JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트

```JSON
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

JSON의 키 &rarr; 반드시 큰따옴표로 묶어야 함  
JSON의 값 &rarr; 객체 리터럴과 같은 표기법을 그대로 사용 가능, 하지만 문자열은 큰따옴표로 묶어야 함

### 43.2.2 JSON.stringify

JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환  
직렬화: 클라이언트가 서버로 객체를 전송하기 위해 객체를 문자열화

```js
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 반환
const json = JSON.stringify(obj);
console.log(typeof json, json);

// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}
```

JSON.stringify 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환

### 43.2.3 JSON.parse

JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환  
역직렬화: 서버로부터 클라이언트로 전송된 JSON 데이터는 문자열, 이를 사용하기 위한 객체화

```js
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: 'Lee', age: 20, alive: true, hobby: ['traveling', 'tennis']}
```

배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환  
배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환

## 43.3 XMLHttpRequest

XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티 제공

### 43.3.1 XMLHttpRequest 객체 생성

XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성  
브라우저 환경에서만 정상적으로 실행

```js
const xhr = new XMLHttpRequest();
```

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

XMLHttpReqeust 객체는 다양한 프로퍼티와 메서드 제공

#### XMLHttpRequest 객체의 프로토타입 프로퍼티

| 프로토타입 프로퍼티 | 설명                                           |
| :------------------ | :--------------------------------------------- |
| **readyState**      | HTTP 요청의 현재 상태를 나타내는 정수          |
| **status**          | HTTP 요청에 대한 응답 상태를 나타내는 정수     |
| **statusText**      | HTTP 요청에 대한 응답 메시지를 나타내는 문자열 |
| **responseType**    | HTTP 응답 타입                                 |
| **response**        | HTTP 요청에 대한 응답 몸체                     |
| responseText        | 서버가 전송한 HTTP 요청에 대한 응답 문자열     |

#### XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

| 이벤트 핸들러 프로퍼티 | 설명                                                         |
| :--------------------- | :----------------------------------------------------------- |
| **onreadystatechange** | readyState 프로퍼티 값이 변경된 경우                         |
| onloadstart            | HTTP 요청에 대한 응답을 받기 시작한 경우                     |
| onprogress             | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생            |
| onabort                | abort 메서드에 의해 HTTP 요청이 중단된 경우                  |
| **onerror**            | HTTP 요청에 에러가 발생한 경우                               |
| **onload**             | HTTP 요청이 성공적으로 완료한 경우                           |
| ontimeout              | HTTP 요청 시간이 초과한 경우                                 |
| onloadend              | HTTP 요청이 완료한 경우, HTTP 요청이 성공 또는 실패하면 발생 |

#### XMLHttpRequest 객체의 메서드

| 메서드               | 설명                                     |
| :------------------- | :--------------------------------------- |
| **oepn**             | HTTP 요청 초기화                         |
| **send**             | HTTP 요청 전송                           |
| **abort**            | 이미 전송된 HTTP 요청 중단               |
| **setRequestHeader** | 특정 HTTP 요청 헤더의 값을 설정          |
| getResponseHeader    | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

#### XMLHttpRequest 객체의 정적 프로퍼티

| 정적 프로퍼티    | 값  | 설명                                  |
| :--------------- | :-: | :------------------------------------ |
| UNSENT           |  0  | open 메서드 호출 이전                 |
| OPENED           |  1  | open 메서드 호출 이후                 |
| HEADERS_RECEIVED |  2  | send 메서드 호출 이후                 |
| LOADING          |  3  | 서버 응답 중(응답 데이터 미완성 상태) |
| **DONE**         |  4  | 서버 응답 완료                        |

### 43.3.3 HTTP 요청 전송

HTTP 요청을 전송하는 경우 다음 순서를 따름

1. XMLHttpRequest.prototype.open 매서드로 HTTP 요청을 초기화
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send();
```

#### XMLHttpRequest.prototype.open

open 메서드는 서버에 전송할 HTTP 요청을 초기화  
`xhr.open(method, url[, async])`

| 매개변수 | 설명                                                |
| :------- | :-------------------------------------------------- |
| method   | HTTP 요청 메서드("GET", "POST", "PUT", "DELETE" 등) |
| url      | HTTP 요청을 전송할 URL                              |
| async    | 비동기 요청 여부(기본값 true, 비동기 방식으로 동작) |

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법  
주로 5가지 요청 메서드를 사용하여 CRUD를 구현

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| :--------------- | :------------- | :-------------------- | :------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

#### XMLHttpRequest.prototype.send

send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송  
기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있음

- GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송
- POST 요청 메서드의 경우 데이터(페이로드)를 요청 몸체에 담아 전송

#### XMLHttpRequest.prototype.setRequestHeader

setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정  
setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출 필요

자주 사용하는 HTTP 요청 헤더인 Content-type, Accept

| MIME 타입   | 서브타입                                           |
| :---------- | :------------------------------------------------- |
| text        | text/plain, text/html, text/css, text/javascript   |
| application | application/json, application/x-www-form-urlencode |
| multipart   | multipart/formed-data                              |

### 43.3.4 HTTP 응답 처리

서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야함
