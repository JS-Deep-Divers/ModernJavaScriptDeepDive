# 37장. Set과 Map

## 37.1 Set

Set 객체는 중복되지 않는 유일한 값들의 집합

배열과 Set 객체의 차이

|                구분                 | 배열 | Set객체 |
| :---------------------------------: | :--: | :-----: |
| 동일한 값을 중복하여 포함할 수 있음 |  O   |    X    |
|       요소 순서에 의미가 있음       |  O   |    X    |
|      인덱스로 요소에 접근 가능      |  O   |    X    |

Set은 수학적 집합을 구현하기 위한 자료구조  
이를 통해 교집합, 합집합, 차집합, 여집합 등 구현 가능

### 37.1.1 Set 객체의 생성

Set 객체는 Set 생성자 함수로 생성

```js
const set = new Set();
console.log(set); // Set(0) {}
```

Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성  
이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않음

```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}
```

배열에서 중복된 요소 제거 가능

### 37.1.2 요소 개수 확인

Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티 사용

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 37.1.3 요소 추가

Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드 사용

```js
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

add 메서드는 새로운 요소가 추가된 Set 객체를 반환, 연속적으로 호출 가능  
Set 객체에 중복된 요소의 추가는 허용되지 않음(에러 발생 무시)  
Set 객체는 객체, 배열과 같이 자바스크립트의 모든 값을 요소로 저장 가능

### 37.1.4 요소 존재 여부 확인

Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용  
has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 37.1.5 요소 삭제

Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용  
delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환  
인수로 인덱스가 아니라 삭제하려는 요소값 전달 필요  
Set 객체는 배열과 같이 인덱스를 같이 않음

```js
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set); // Set(2) {1, 3}

set.delete(1);
console.log(set); // Set(1) {3}
```

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환, 연속적으로 호출 불가  
존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시

### 37.1.6 요소 일괄 삭제

Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용  
clear 메서드는 언제나 undefined 반환

```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### 37.1.7 요소 순회

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용  
Set.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달

콜백 함수가 전달받는 3개의 인수

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Set 객체 자체

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

Set 객체는 이터러블, for ... of 문 순회, 스프레드 문법, 배열 디스트럭처링 할당의 대상이 될 수 있음  
Set 객체는 요소의 순서에 의미를 갖지 않으나 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따름

### 37.1.8 집합 연산

Set 객체는 수학적 집합을 구현하기 위한 자료구조  
이를 통해 교집합, 합집합, 차집합 등 구현 가능

## 37.2 Map

Map 객체는 키와 값의 쌍으로 이루어진 컬렉션

객체와 Map 객체의 차이

|          구분          |          객체           |       Map 객체        |
| :--------------------: | :---------------------: | :-------------------: |
| 키로 사용할 수 있는 값 |   문자열 또는 심벌 값   | 객체를 포함한 모든 값 |
|        이터러블        |            X            |           O           |
|     요소 개수 확인     | Object.keys(obj).length |       map.size        |

### 37.2.1 Map 객체의 생성

Map 객체는 Map 생성자 함수로 생성

```js
const map = new Map();
console.log(map); // Map(0) {}
```

Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성  
이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 함  
Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없음

### 37.2.2 요소 개수 확인

Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티 사용

```js
const { size } = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(size); // 2
```

### 37.2.3 요소 추가

Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "value1");
console.log(map); // Map(1) {"key1" => "value1"}
```

set 메서드는 새로운 요소가 추가된 Map 객체를 반환, 연속적 호출 가능  
Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어 써짐(에러 발생 무시)

객체는 문자열 또는 심벌 값만 키로 사용 가능  
Map 객체는 키 타입에 제한이 없음(객체를 포함한 모든 값을 키로 사용 가능)

### 37.2.4 요소 취득

Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용  
get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환  
인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined 반환

```js
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.get(lee)); // developer
console.log(map.get("key")); // undefined
```

### 37.2.5 요소 존재 여부 확인

Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드 사용  
has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환

```js
const lee = { name: "Lee" };
const kim = { name: "kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

### 37.2.6 요소 삭제

Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용  
delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환

```js
const lee = { name: "Lee" };
const kim = { name: "kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```

만약 존재하지 않는 키로 Map 객체의 요소를 삭제하려 하면 에러 없이 무시
delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환, 연속적 호출 불가

### 37.2.7 요소 일괄 삭제

Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용
clear 메서드는 언제나 undefined 반환

```js
const lee = { name: "Lee" };
const kim = { name: "kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.clear();
console.log(map); // Map(0) {}
```

### 37.2.8 요소 순회

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용

Map.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달

콜백 함수가 전달받는 3개의 인수

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소키
- 세 번째 인수: 현재 순회 중인 Map 객체 자체

```js
const lee = { name: "Lee" };
const kim = { name: "kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: 'Lee'} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: 'kim'} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

Map 객체는 이터러블, for ... of 문 순회, 스프레드 문법, 배열 디스트럭처링 할당의 대상이 될 수 있음  
Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공

| Map 메서드            | 설명                                                                                      |
| :-------------------- | :---------------------------------------------------------------------------------------- |
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환          |
| Map.prototype.values  | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환          |
| Map.prototype.entries | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |

Map 객체는 요소의 순서에 의미를 갖지 않으나 Map 객체를 순회하는 순서는 요소가 추가된 순서를 따름

# 42장. 비동기 프로그래밍

## 42.1 동기 처리와 비동기 처리

함수를 호출하면 함수코드가 평가되어 함수 실행 컨텍스트가 생성  
이때 생성된 함수 실행 컨텍스트는 실행 컨텍스트 스택(콜 스택)에 푸시되고 함수 코드가 실행  
함수 코드의 실행이 종료하면 함수 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거  
함수의 실행 순서는 실행 컨텍스트 스택으로 관리

자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 보유  
자바스크립트 엔진은 한 번에 하나의 태스크만 실행할 수 있는 **싱글 스레드** 방식으로 동작  
싱글 스레드 방식은 한 번에 하나의 태스크만 실행할 수 있기 때문에 처리에 시간이 걸리는 태스크를 실행하는 경우 **블로킹(작업 중단)** 이 발생

```js
// sleep 함수는 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출
function sleep(func, delay) {
  // Date.now()는 현재 시간을 숫자(ms)로 반환
  const delayUntil = Date.now() + delay;

  // 현재 시간(Date.now())에 delay를 더한 delayUntil이 현재 시간보다 작으면 계속 반복
  while (Date.now() < delayUntil);
  // 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출
  func();
}

function foo() {
  console.log("foo");
}

function bar() {
  console.log("bar");
}

// sleep 함수는 3초 이상 실행
sleep(foo, 3 * 1000);
// bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로 3초 이상 블로킹
bar();
// (3초 경과 후) foo 호출 -> bar 호출
```

sleep 함수는 3초 후에 foo 함수를 호출  
이때 bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로 3초 이상 호출되지 못하고 블로킹

**동기 처리**: 현재 실행 중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식을

- 장점: 태스크를 순서대로 하나씩 처리하므로 실행 순서가 보장
- 단점: 앞선 태스크가 종료할 때까지 이후 태스크들이 블로킹

setTimeout을 사용해 수정

```js
function foo() {
  console.log("foo");
}

function bar() {
  console.log("bar");
}

// 타이머 함수 setTimeout은 일정 시간이 경과한 이후에 콜백 함수 foo를 호출
// 타이머 함수 setTimeout은 bar 함수를 블로킹하지 않음
setTimeout(foo, 3 * 1000);
bar();
// bar 호출 -> (3초 경과 후)foo 호출
```

setTimeout 함수 이후의 태스크를 블로킹하지 않고 곧바로 실행

**비동기 처리**: 현재 실행중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식

- 장점: 현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행, 블로킹 발생하지 않음
- 단점: 태스크의 실행 순서가 보장되지 않음

비동기 처리를 수행하는 비동기 함수는 전통적으로 콜백 패턴을 사용  
비동기 처리를 위한 콜백 패턴은 콜백 헬을 발생시켜 가독성 &darr;  
비동기 처리 중 발생한 에러의 예외 처리가 곤란  
여러 개의 비동기 처리르 랗ㄴ 번에 처리하는 데 한계 존재

타이머 함수인 setTimeout과 setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작

## 42.2 이벤트 루프와 태스크 큐

브라우저가 동작하는 것을 살펴보면 많은 태스크가 동시에 처리되는 것 처럼 느껴짐  
자바스크립트의 동시성을 지원하는 것이 바로 **이벤트 루프**

이벤트 루프는 브라우저에 내장되어 있는 기능 중 하나  
대부분의 자바스크립트 엔진은 크게 2개의 영역으로 구분

- **콜 스택**  
  소스코드(전역 코드나 함수 코드 등) 평가 과정에서 생성된 실행 컨텍스트가 추가되고 제거되는 스택 자료구조인 실행 컨텍스트 스택  
  함수 호출 시, 함수 실행 컨텍스트가 순차적으로 콜 스택에 푸시되어 순차적으로 실행  
  자바스크립트 엔진은 단 하나의 콜 스택 사용  
  최상위 실행 컨텍스트가 종료되어 콜 스택에서 제거되기 전까지는 다른 어떤 태스크도 실행되지 않음
- **힙**  
  객체가 저장되는 메모리 공간  
  콜 스택의 요소인 실행 컨텍스트는 힙에 저장된 객체를 참조  
  메모리에 값을 저장하려면 저장할 메모리 공간의 크기 결정 필요  
  객체는 크기가 정해져 있지 않으므로 메모리 공간의 크기를 런타임에 결정(동적 할당)  
  객체가 저장되는 메모리 공간인 힙은 구조화 되어 있지 않음

비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 자바스크립트 엔진을 구동하는 환경인 브라우저 또는 Node.js가 담당  
이를 위해 브라우저 환경은 태스크 큐와 이벤트 루프 제공

- **태스크 큐**  
  비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역
- **이벤트 루프**  
  콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 태스크 큐에 대기 중인 함수(콜백 함수, 이벤트 핸들러 등)가 있는지 반복해서 확인  
  만약 콜 스택이 비어 있고 태스크 큐에 대기 중인 함수가 있다면,  
  &rarr; 이벤트 루프는 순차적으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동, 콜 스택으로 이동한 함수는 실행  
  태스크 큐에 일시 보관된 함수들은 비동기 처리 방식으로 동작

foo 함수와 bar 함수 중에서 먼저 실행될 함수는?

```js
function foo() {
  console.log("foo");
}

function bar() {
  console.log("bar");
}

setTimeout(foo, 0); // 0초(실제는 4ms) 후에 foo 함수가 호출
bar();
```

비동기 함수인 setTimeout의 콜백 함수는 태스크 큐에 푸시되어 대기하다가 콜 스택이 비게 되면 콜 스택에 푸시되어 실행  
자바스크립트(브라우저가 아닌 브라우저에 내장된 자바스크립트 엔진)는 싱글 스레드 방식으로 동작

- 자바스크립트 엔진 &rarr; 싱글 스레드로 동작
- 브라우저 &rarr; 멀티 스레드로 동작
