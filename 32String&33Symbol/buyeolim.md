# 32장. String

표준 빌트인 객체인 String은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드 제공

## 32.1 String 생성자 함수

표준 빌트인 객체인 String 객체는 생성자 함수 객체  
new 연산자와 함께 호출하여 String 인스턴스 생성 가능

- String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 \[\[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체 생성
  ```js
  const strObj = new String();
  console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}
  ```
- String 생성자 함수에 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 \[\[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체 생성
  ```js
  const strObj = new String("Lee");
  console.log(strObj); // String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}
  ```

String 래퍼 객체

- 프로퍼티 키: length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열
- 프로퍼티 값: 각 문자

String 래퍼 객체는 유사 배열 객체이면서 이터러블  
&rarr; 배열과 유사하게 인덱스를 사용하여 각 문자에 접근 가능

new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열 반환  
이를 이용하여 명시적으로 타입 변환

```js
String(1); // -> "1"
String(true); // -> "true"
```

## 32.2 length 프로퍼티

문자열의 문자 개수를 반환

```js
"Hello".length; // -> 5
```

## 32.3 String 메서드

참고. 배열 메서드는 두 종류로 구분 가능

- 원본 배열을 직접 변경하는 메서드
- 새로운 배열을 생성하여 변환하는 메서드

하지만 String 객체의 메서드는 언제나 새로운 문자열을 반환  
String 래퍼 객체도 읽기 전용 객체로 제공(문자열은 변경 불가능한 원시 값)

### 32.3.1 String.prototype.indexOf

indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색

- 성공: 첫 번째 인덱스를 반환
- 실패: -1 반환

```js
const str = "Hello World";

str.indexOf("l"); // -> 2
str.indexOf("or"); // -> 7
str.indexOf("x"); // -> -1

// indexOF 메서드의 2번째 인수: 검색을 시작할 인덱스
str.indexOf("l", 3); // -> 3
```

대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용

```js
if (str.indexOf("Hello") !== -1) {
  // 문자열 str에 'Hello'가 포함되어 있는 경우 처리
}

// ES6 도입, String.prototype.includes 메서드
if (str.includes("Hello")) {
  // 문자열 str에 'Hello'가 포함되어 있는 경우 처리
}
```

### 32.3.2 String.prototype.search

search 메서드는 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색

- 성공: 문자열의 인덱스 반환
- 실패: -1 반환

```js
const str = "Hello world";

str.search(/o/); // -> 4
str.search(/x/); // -> -1
```

### 32.3.3 String.prototype.includes

ES6 도입  
includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인

- 성공: true 반환
- 실패: false 반환

```js
const str = "Hello world";

str.includes("Hello"); // -> true
str.includes(" "); // -> true
str.includes("x"); // -> false
str.includes(); // -> false

// includes 메서드의 2번째 인수: 검색을 시작할 인덱스
str.includes("l", 3); // -> true
```

### 32.3.4 String.prototype.startsWith

ES6 도입  
startsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인

- 성공: true 반환
- 실패: false 반환

```js
const str = "Hello world";

str.startsWith("He"); // -> true
str.startsWith("x"); // -> false

// startsWith 메서드의 2번째 인수: 검색을 시작할 인덱스
str.startsWith(" ", 5); // -> true
```

### 32.3.5 String.prototype.endsWith

ES6에서 도입  
endsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인

- 성공: true 반환
- 실패: false 반환

```js
const str = "Hello world";

str.endsWith("ld"); // -> true
str.endsWith("x"); // -> false

// endsWith 메서드의 2번째 인수: 검색을 시작할 인덱스
str.endsWith("lo", 5); // -> true
```

### 32.3.6 String.prototype.charAt

chatAt 메서드는 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환, 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환

```js
const str = "Hello";

for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i)); // H e l l o
}
```

유사한 문자열 메서드

- String.prototype.charCodeAt
- String.prototype.codePointAt

### 32.3.7 String.prototype.substring

substring 메서드는 대상 문자열에서 첫 번재 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열 반환

```js
const str = "Hello world";

str.substring(1, 4); // -> 'ell'

// 두 번째 인수 생략 시, 마지막 문자까지 부분 문자열 반환
str.substring(1); // -> 'ello World'
```

substring 메서드의 첫 번재 인수는 두 번째 인수보다 작은 정수이어야 정상  
다음과 같이 인수를 전달한 경우에도 정상 작동

- 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환
- 인수 < 0 또는 NaN인 경우 0으로 취급
- 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급

```js
const str = "Hello world";

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환
str.substring(4, 1); // -> 'ell'

// 인수 < 0 또는 NaN인 경우 0으로 취급
str.substring(-2); // -> 'Hello world'

// 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급
str.substring(1, 100); // -> 'ello World'
str.substirng(20); // -> ''
```

### 32.3.8 String.prototype.slice

slice 메서드는 substring 메서드와 동일하게 동작  
단, slice 메서드에는 음수인 인수 전달 가능  
음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환

```js
const str = "Hello world";

// substring과 slice 메서드는 동일하게 동작
str.substring(0, 5); // -> 'hello'
str.slice(0, 5); // -> 'hello'

str.substring(2); // -> 'llo World'
str.slice(2); // -> 'llo World'

// slice 메서드는 음수인 인수를 전달 가능
str.substring(-5); // -> 'hello World'
str.slice(-5); // -> 'World'
```

### 32.3.9 String.prototype.toUpperCase

toUpperCase 메서드는 대상 문자열을 모두 대문자로 변경한 문자열을 반환

```js
const str = "Hello world";

str.toUpperCase(); // -> 'HELLO WORLD!'
```

### 32.3.10 String.prototype.toLowerCase

toLowerCase 메서드는 대상 문자열을 모두 대문자로 변경한 문자열을 반환

```js
const str = "Hello world";

str.toLowerCase(); // -> 'hello world!'
```

### 32.3.11 String.prototype.trim

trim 메서드는 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환

```js
const str = "   foo  ";

str.trim(); // -> 'foo'
```

String.prototype.trimStart, String.prototype.trimEnd 메서드도 사용 가능

```js
const str = "   foo  ";

str.trimStart(); // -> 'foo  '
str.trimEnd(); // -> '   foo'
```

### 32.3.12 String.prototype.repeat

ES6 도입  
repeat 메서드는 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환

- 인수로 전달받은 정수가 0이면 빈 문자열 반환
- 인수로 전달받은 정수가 음수면 RangeError 발생
- 인수를 생략하면 기본값 0 설정

```js
const str = "abc";

str.repeat(); // -> ''
str.repeat(0); // -> ''
str.repeat(1); // -> 'abc'
str.repeat(2); // -> 'abcabc'
str.repeat(2.5); // -> 'abcabc' (2.5 -> 2)
str.repeat(-1); // -> RangeError: Invalid count value: -1
```

### 32.3.13 String.prototype.replace

replace 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환

```js
const str = "Hello world world";

str.replace("world", "Lee"); // -> 'Hello Lee world'
```

### 32.3.14 String.prototype.split

split 메서드는 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환  
인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환

```js
const str = "How are you doing?";

str.split(" "); // -> ['How', 'are', 'you', 'doing?']
str.split(/\s/); // -> ['How', 'are', 'you', 'doing?']
str.split(""); // -> ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?']
str.split(); // -> ['How are you doing?']

// 두 번째 인수로 배열의 길이 지정
str.split(" ", 3); // -> ['How', 'are', 'you']
```

# 33장. 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

1997년 자바스크립트가 ECMAScript로 표준화된 이래로 6개의 타입(문자열, 숫자, 불리언, undefined, null, 객체) 존재

**심벌**
ES6 도입  
7번째 데이터 타입으로 변경 불가능한 원시 타입의 값  
심벌 값은 다른 값과 중복되지 않는 유일무이한 값  
주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용

## 33.2 심벌 값의 생성

### 33.2.1 Symbol 함수

심벌 값은 Symbol 함수를 호출하여 생성  
다른 원시값(문자열, 숫자, 불리언, undefined, null 타입의 값)은 리터럴 표기법을 통해 값 생성 가능

생성된 심벌 값은 외부로 노출되지 않아 확인 불가, 다른 값과 절대 중복되지 않는 유일무이한 값

```js
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

console.log(mySymbol); // Symbol()
```

Symbol 함수는 String, Number, Boolean 생성자 함수와는 달리 new 연산자와 함께 호출하지 않음

```js
new Symbol(); // TypeError: Symbol is not a constructor
```

Symbol 함수에는 선택적으로 문자열을 인수로 전달 가능  
이 문자열을 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용, 심벌 값 생성에 어떠한 영향도 주지 않음  
심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값

```js
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성  
심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않음, 불리언 타입으로는 암묵적으로 타입 변환

### 33.2.2 Symbol.for / Symbol.keyFor 메서드

Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
- 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출 가능

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않음
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // -> undefined
```

## 33.3 심벌과 상수

상수를 이용한 정의

```js
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};

const myDirection = Direction.UP; // 변수에 상수를 할당

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

이때 상수 값 1, 2, 3, 4가 변경될 수 있으며 다른 변수 값과 중복될 수 있음

이러한 경우 변경/중복될 가능성이 있는 무의미한 상수대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용 가능

```js
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

## 33.4 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성 가능

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // -> 1
```

심벌 값은 유일무이한 값, 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 충돌 불가

## 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없음  
심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉 가능

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않음
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

하지만 ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티 찾기 가능

## 33.6 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않음  
표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋음

```js
// 표준 빌트인 객체를 확장하는 것은 권장하지 않음
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].sum(); // -> 3
```

개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문

중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 문제 해결 가능

```js
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for("sum")](); // -> 3
```

## 33.7 Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서 부르는 이름  
Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용
