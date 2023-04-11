# 28장. Number

표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티 및 메서드 제공

## 28.1 Number 생성자 함수

Number 객체는 생성자 함수 객체, new 연산자와 함께 호출하여 Number 인스턴스 생성 가능

Number 생성자 함수의 인수 전달

- 인수 전달하지 않음  
   \[\[NumberData]] 내부 슬롯에 인수로 0을 할당한 Number 래퍼 객체 생성
  ```js
  const numObj = new Number();
  console.log(numObj); // Number {[[PrimitiveValue]]: 0}
  ```
- 숫자  
  \[\[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체 생성
  ```js
  const numObj = new Number(10);
  console.log(numObj); // Number {[[PrimitiveValue]]: 10}
  ```
- 숫자가 아닌 값  
  인수를 숫자로 강제 변환 후, \[\[NumberData]] 내부 슬롯에 숫자를 할당한 Number 객체 생성  
  변환 불가 시, \[\[NumberData]] 내부 슬롯에 NaN을 할당한 Number 객체 생성

  ```js
  let numObj = new Number("10");
  console.log(numObj); // Number {[[PrimitiveValue]]: 10}

  numObj = new Number("Hello");
  console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
  ```

new 연산자를 사용하지 않고 Number 생성자 함수를 호출 시, Number 인스턴스가 아닌 숫자를 반환(명시적 타입 변환 사용 가능)

```js
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
Number(true); // -> 1
Number(false); // -> 0
```

## 28.2 Number 프로퍼티

### 28.2.1 Number.EPSILON

ES6 도입
1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 동일  
약 $2.220446049250313 \times 10^{-16}$의 값

부동소수점 산술 연산은 정확한 결과를 기대하기 어려움  
부동소수점을 표현하기 위해 가장 널리 쓰이는 표준 IEEE 754는 2진법으로 변환했을 때 무한소수가 되어 미세한 오차 발생

```js
0.1 + 0.2; // -> 0.30000000000000004
0.1 + 0.2 === 0.3; // -> false
```

Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용

```js
function isEqual(a, b) {
  // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // -> true
```

### 28.2.2 Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수 값($1.7976931348623157 \times 10^{308}$)  
Number.MAX_VALUE 보다 큰 숫자는 Infinity

```js
Number.MAX_VALUE; // -> 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // -> true
```

### 28.2.3 Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양수 값($5 \times 10^{-324}$)  
Number.MIN_VALUE보다 작은 숫자는 0

```js
Number.MIN_VALUE; // -> 5e-324
Number.MIN_VALUE > 0; // -> true
```

### 28.2.4 Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값($9007199254740991$)

### 28.2.5 Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값($-9007199254740991$)

### 28.2.6 Number.POSITIVE_INFINITY

양의 무한대를 나타내는 숫자값 Infinity

### 28.2.7 Number.NEGATIVE_INFINITY

양의 무한대를 나타내는 숫자값 -Infinity

### 28.2.8 Number.NaN

숫자가 아님(Not-a-Number)을 나타내는 숫자값  
Number.NaN은 window.NaN과 동일

## 28.3 Number 메서드

### 28.3.1 Number.isFinite

ES6 도입  
인수로 전달된 숫자값이 정상적인 유한수(Infinity 또는 -Infinity)가 아닌지 검사하여 불리언 값으로 반환  
인수가 NaN이면 언제나 false

```js
Number.isFinite(0); // -> true
Number.isFinite(Number.MAX_VALUE); // -> true
Number.isFinite(Number.MIN_VALUE); // -> true
Number.isFinite(Infinity); // -> false
Number.isFinite(-Infinity); // -> false
```

Number.isFinite 메서드와 빌트인 전역 함수 isFinite의 차이

- 빌트인 전역 함수 isFinite  
  &rarr; 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사 수행
- Number.isFinite  
  &rarr; 전달받은 신수를 숫자로 암묵적 타입 변환하지 않음, 숫자가 아닌 인수일 때 반환값 false

```js
Number.isFinite(null); // -> false
isFinite(null); // -> true
```

### 28.3.2 Number.isInteger

ES6 도입  
인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환  
검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않음

```js
Number.isInteger(123); // -> true
Number.isInteger("123"); // -> false
Number.isInteger(false); // -> false
Number.isInteger(Infinity); // -> false
```

### 28.3.3 Number.isNaN

ES6 도입  
인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환

Number.isNaN 메서드와 빌트인 전역함수 isNaN의 차이

- 빌트인 전역함수 isNaN  
  &rarr; 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사 수행
- Number.isNaN 메서드  
  &rarr; 전달받은 인수를 숫자로 암묵적 타입 변환하지 않음, 숫자가 아닌 인수일 때 반환값 false

```js
Number.isNaN(undefined); // -> false
isNaN(undefined); // -> true
```

### 28.3.4 Number.isSafeInteger

ES6 도입  
인수로 전달된 숫자값이 안전한 정수($-(2^{53} - 1)$과 $2^{53} - 1$사이의 정수값)인지 검사하여 그 결과를 불리언 값으로 반환  
검사전에 인수를 숫자로 암묵적 타입 변환하지 않음

```js
Number.isSafeInteger(0); // -> true
Number.isSafeIntegerJ(0.5); // -> false
Number.isSafeIntegerJ("123"); // -> false
Number.isSafeIntegerJ(Infinity); // -> false
```

### 28.3.5 Number.prototype.toExponential

숫자를 지수 표기법으로 변환하여 문자열로 반환

**지수 표기법**  
매우 크거나 작은 숫자를 표기할 때 주로 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식  
인수로 소수점 이하로 표현할 자릿수 전달

```js
(77.1234).toExponential(); // -> "7.71234e+1"
(77.1234).toExponential(4); // -> "7.7123e+1"
(77.1234).toExponential(2); // -> "7.71e+1"
```

숫자 뒤의 . 은 의미가 모호(부동 소수점 숫자의 소수 구분 기호, 객체 프로퍼티에 접근하기 위한 프로퍼티 접근 연산자 등)  
자바스크립트 엔진은 숫자 뒤의 . 을 부동 소수점 숫자의 소수 구분 기호로 해석  
숫자는 Number의 래퍼 객체이므로 . 뒤에 오는 toExponential을 프로퍼티로 해석할 수 없음

```js
77.toExponential();      // SyntaxError: Invalid or unexpected token
77.1234.toExponential(); // -> "7.71234e+1", 두 번째 .은 프로퍼티 접근 연산자
(77).toExponential();    // -> "7.7e+1"
77 .toExponential();     // -> "7.7e+1" 공백 뒤 .은 프로퍼티 접근 연산자
```

혼란 방지를 위해 그룹 연산자 사용을 권장

### 28.3.6 Number.prototype.toFixed

숫자를 반올림하여 문자열로 반환  
반올림하는 소수점 이하 자릿수를 나타내는 0 ~ 20 사이의 정수값을 인수로 전달 가능  
인수 생략 시, 기본값 0 지정

```js
(12345.6789).toFixed(); // -> "123456"
(12345.6789).toFixed(1); // -> "12345.7"
(12345.6789).toFixed(2); // -> "12345.68"
(12345.6789).toFixed(3); // -> "12345.679"
```

### 28.3.7 Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환  
인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환  
인수 범위는 0 ~ 21 사이의 정수값, 생략 시 기본값 0

```js
(12345.6789).toPrecision(); // -> "12345.678"
(12345.6789).toPrecision(1); // -> "1e+4"
(12345.6789).toPrecision(2); // -> "1.2e+4"
(12345.6789).toPrecision(6); // -> "12345.7"
```

### 28.3.8 Number.prototype.toString

숫자를 문자열로 변환하여 반환  
인수 범위는 진법을 나타내는 2 ~ 36 사이의 정수값, 생략 시 기본값 10

```js
(10).toString(); // -> "10"
(16).toString(2); // -> "10000"
(16).toString(8); // -> "20"
(16).toString(16); // -> "10"
```

# 29장. Math

표준 빌트인 객체인 Math는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공  
Math는 생성자 함수가 아니므로 정적 프로퍼티와 정적 메서드만 제공

## 29.1 Math 프로퍼티

### 29.1.1 Math.PI

원주율 PI 값($\pi \approx 3.141592653589793$)을 반환

## 29.2 Math 메서드

### 29.2.1 Math.abs

인수로 전달된 숫자의 절대값을 반환  
절대값은 반드시 0 또는 양수

```js
Math.abs(-1); // -> 1
Math.abs("-1"); // -> 1
Math.abs(""); // -> 0
Math.abs("[]"); // -> 0
Math.abs(null); // -> 0
Math.abs(undefined); // -> NaN
Math.abs({}); // -> NaN
Math.abs("string"); // -> NaN
MAth.abs(); // -> NaN
```

### 29.2.2 Math.round

인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환

```js
Math.round(1.4); // -> 1
Math.round(1.6); // -> 2
Math.round(-1.4); // -> -1
Math.round(-1.6); // -> -2
Math.round(1); // -> 1
Math.round(); // -> NaN
```

### 29.2.3 Math.ceil

인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환  
소수점 이하를 올림하면 더 큰 정수

```js
Math.ceil(1.4); // -> 2
Math.ceil(1.6); // -> 2
Math.ceil(-1.4); // -> -1
Math.ceil(-1.6); // -> -1
Math.ceil(1); // -> 1
Math.ceil(); // -> NaN
```

### 29.2.4 Math.floor

인수로 전달됫 숫자의 소수점 이하를 내림한 정수를 반환  
소수점 이하를 내림하면 더 작은 정수

```js
Math.floor(1.9); // -> 1
Math.floor(9.1); // -> 9
Math.floor(-1.9); // -> -2
Math.floor(-9.1); // -> -10
Math.floor(1); // -> 1
Math.floor(); // -> NaN
```

### 29.2.5 Math.sqrt

인수로 전달된 숫자의 제곱근을 반환

```js
Math.sqrt(9); // -> 3
Math.sqrt(-9); // -> NaN
Math.sqrt(2); // -> 1.4142135623730951
Math.sqrt(1); // -> 1
Math.sqrt(0); // -> 0
Math.sqrt(); // -> NaN
```

### 29.2.6 Math.random

임의의 난수(랜덤 숫자)를 반환  
반환난 난수는 0에서 1미만의 실수(0 포함, 1 미포함)

```js
Math.random(); // 0에서 1 미안의 랜덤 실수

/*
1에서 10 범위의 랜덤 정수 취득
1) Math.random으로 0에서 1미만의 랜덤 실수를 구한 다음, 10을 곱해 0에서 10 미만의 랜덤 실수를 구함
2) 0에서 10 미만의 랜덤 실수에 1을 더해 1에서 10 범위의 랜덤 실수를 구함
3) Math.floor로 1에서 10 범위의 랜덤 실수의 소수점 이하를 떼어 버린 다음 정수 반환
*/
const random = Math.floor(Math.random() * 10 + 1);
console.log(random); // 1에서 10 범위의 정수
```

### 29.2.7 Math.pow

첫 번째 인수를 밑, 두 번째 인수를 지수로 거듭제곱한 결과를 반환

```js
Math.pow(2, 8); // -> 256
Math.pow(2, -1); // -> 0.5
Math.pow(2); // -> NaN
```

ES7 도입된 지수 연산자를 사용하면 가독성이 더 좋음

```js
// ES7 지수 연산자
2 ** (2 ** 2); // -> 16
Math.pow(Math.pow(2, 2), 2); // -> 16
```

### 29.2.8 Math.max

전달받은 인수 중에서 가장 큰 수를 반환  
인수가 전달되지 않으면 -Infinity를 반환

```js
Math.max(1); // -> 1
Math.max(1, 2); // -> 2
Math.max(1, 2, 3); // -> 3
Math.max(); // -> -Infinity
```

배열을 인수로 전달받아 배열의 요소 중에서 최대값 구하기

- Function.prototype.apply 메서드
  ```js
  Math.max.apply(null, [1, 2, 3]); // -> 3
  ```
- 스프레드 문법(ES6)
  ```js
  Math.max(...[1, 2, 3]); // -> 3
  ```

### 29.2.9 Math.min

전달받은 인수 중에서 가장 작은 수를 반환  
인수가 전달되지 않으면 Infinity를 반환

```js
Math.min(1); // -> 1
Math.min(1, 2); // -> 1
Math.min(1, 2, 3); // -> 1
Math.min(); // -> Infinity
```

배열을 인수로 전달받아 배열의 요소 중에서 최소값 구하기

- Function.prototype.apply 메서드
  ```js
  Math.min.apply(null, [1, 2, 3]); // -> 1
  ```
- 스프레드 문법(ES6)
  ```js
  Math.min(...[1, 2, 3]); // -> 1
  ```

# 30장. Date

표준 빌트인 객체인 Date는 날짜와 시간(연, 월, 일, 시, 분, 초, 밀리초)을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수

UTC(협정 세계시)는 국제 표준시, GMT(그리니치 평균시)로 불리기도 함  
일상에서는 혼용되어 사용, 기술적인 표기에서는 UTC가 사용  
KST(한국 표준시)는 UTC에 9시간을 더한 시간  
현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정

## 30.1 Date 생성자 함수

Date는 생성자 함수  
Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가짐  
이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 객체가 나타내는 날짜와 시간까지의 밀리초

Date 생성자 함수로 객체를 생성하는 방법 4가지

1. new Date()
2. new Date(milliseconds)
3. new Date(dateString)
4. new Date(year, month\[, day, hour, minute, second, millisecond])

### 30.1.1 new Date()

Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체 반환

```js
new Date(); // -> Mon Apr 03 2023 15:15:50 GMT+0900 (Korean Standard Time)
```

Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환

```js
Date(); // -> 'Mon Apr 03 2023 15:16:43 GMT+0900 (Korean Standard Time)'
```

### 30.1.2 new Date(milliseconds)

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환

```js
new Date(0); // -> Thu Jan 01 1970 09:00:00 GMT+0900 (Korean Standard Time)

/*
86400000ms는 1day를 의미함
1s = 1,000ms
1m = 60s * 1,000ms = 60,000ms
1h = 60m * 60,000ms = 3,600,000ms
1d = 24h * 3,600,000ms = 86,400,000ms
*/
new Date(86400000); // -> Fri Jan 02 1970 09:00:00 GMT+0900 (Korean Standard Time)
```

### 30.1.3 new Date(dateString)

Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환  
인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식 이어야함

```js
new Date("May 26, 2020 10:00:00");
// Tue May 26 2020 10:00:00 GMT+0900 (Korean Standard Time)

new Date("2020/03/26/10:00:00");
// Thu Mar 26 2020 10:00:00 GMT+0900 (Korean Standard Time)
```

### 30.1.4 new Date(year, month\[, day, hour, minute, second, millisecond])

Date 생성자 함수에 연, 월, 일, 시 , 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환  
연, 월은 반드시 지정 필요, 지정하지 않은 옵션 정보는 0 또는 1로 초기화

| 인수        | 내용                                                              |
| :---------- | :---------------------------------------------------------------- |
| year        | 연을 나타내는 1900년 이후의 정수. 0부터 99는 1900부터 1999로 처리 |
| month       | 월을 나타내는 0 ~ 11까지의 정수(주의: 0부터 시작, 0 = 1월)        |
| day         | 일을 나타내는 1 ~ 31까지의 정수                                   |
| hour        | 시를 나타내는 0 ~ 23까지의 정수                                   |
| minute      | 분을 나타내는 0 ~ 59까지의 정수                                   |
| second      | 초를 나타내는 0 ~ 59까지의 정수                                   |
| millisecond | 밀리초를 나타내는 0 ~ 999까지의 정수                              |

```js
new Date(2020, 2);
// -> Sun Mar 01 2020 00:00:00 GMT+0900 (Korean Standard Time)

new Date(2020, 2, 26, 10, 00, 00, 0);
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (Korean Standard Time)

new Date("2020/3/26/10:00:00:00");
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (Korean Standard Time)
```

## 30.2 Date 메서드

### 30.2.1 Date.now

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환

```js
const now = Date.now();
console.log(now); // 1680512124691
```

### 30.2.2 Date.parse

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간 까지의 밀리초를 숫자로 반환

```js
Date.parse("Jan 2, 1970 00:00:00 UTC"); // -> 86400000
```

### 30.2.3 Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환  
new Date(year, month\[, day, hour, minute, second, millisecond])와 같은 형식의 인수 사용  
인수는 UTC로 인식

```js
Date.UTC(1970, 0, 2); // -> 86400000
Date.UTC("1970/1/2"); // -> NaN
```

### 30.2.4 Date.prototype.getFullYear

Date 객체의 연도를 나타내는 정수를 반환

```js
new Date("2020/07/04").getFullYear(); // -> 2020
```

### 30.2.5 Date.prototype.setFullYear

Date 객체에 연도를 나타내는 정수를 설정

```js
const today = new Date();

// 년도 지정
today.setFullYear(2000);
today.getFullYear(); // -> 2000

// 년도/월/일 지정
today.setFullYear(1900, 0, 1);
today.getFullYear(); // -> 1900
```

### 30.2.6 Date.prototype.getMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환  
1월은 0, 12월은 11

```js
new Date("2020/07/24").getMonth(); // -> 6
```

### 30.2.7 Date.prototype.setMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수 설정  
1월은 0, 12월은 11

```js
const today = new Date();

// 월 지정
today.setMonth(0); // 1월
today.getMonth(); // -> 0

// 월/일 지정
today.setMonth(11, 1); // 12월 1일
today.getMonth(); // -> 11
```

### 30.2.8 Date.prototype.getDate

Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 반환

```js
new Date("2020/07/24").getDate(); // -> 24
```

### 30.2.9 Date.prototype.setDate

Date 객체에 날짜(1 ~ 31)를 나타내는 정수를 설정

```js
const today = new Date();

today.setDate(1);
today.getDate(); // -> 1
```

### 30.2.10 Date.prototype.getDay

Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환

|  요일  | 반환값 |
| :----: | :----: |
| 일요일 |   0    |
| 월요일 |   1    |
| 화요일 |   2    |
| 수요일 |   3    |
| 목요일 |   4    |
| 금요일 |   5    |
| 토요일 |   6    |

```js
new Date("2020/07/24").getDay(); // -> 5
```

### 30.2.11 Date.prototype.getHours

Date 객체의 시간(0 ~ 23)을 나타내는 정수를 반환

```js
new Date("2020/07/24/12:00").getHours(); // -> 12
```

### 30.2.12 Date.prototype.setHours

Date 객체에 시간(0 ~ 23)을 나타내는 정수를 설정  
시간 이외 옵션으로 분, 초, 밀리초도 설정 가능

```js
const today = new Date();

// 시간 지정
today.setHours(7);
today.getHours(); // -> 7

// 시간/분/초/밀리초
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // -> 0
```

### 30.2.13 Date.prototype.getMinutes

Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환

```js
new Date("2020/07/24/12:30").getMinutes(); // -> 30
```

### 30.2.14 Date.prototype.setMinutes

Date 객체에 분(0 ~ 59)을 나타내는 정수를 설정  
분 이외에 옵션으로 초, 밀리초도 설정 가능

```js
const today = new Date();

// 분 지정
today.setMinutes(50);
today.getMinutes(); // -> 50

// 분/초/밀리초 지정
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // -> 5
```

### 30.2.15 Date.prototype.getSeconds

Date 객체의 초(0 ~ 59)를 나타내는 정수를 반환

```js
new Date("2020/07/24/12:30:10").getSeconds(); // -> 10
```

### 30.2.16 Date.prototype.setSeconds

Date 객체에 초(0 ~ 59)를 나타내는 정수를 설정  
초 이외에 옵션으로 밀리초도 설정 가능

```js
const today = new Date();

// 초 지정
today.setSeconds(30);
today.getSeconds(); // -> 30

// 초/밀리초 지정
today.setSeconds(10, 0); // HH:MM:10:000
today.getSeconds(); // -> 10
```

### 30.2.17 Date.prototype.getMilliseconds

Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 반환

```js
new Date("2020/07/24/12:30:10:150").getMilliseconds(); // -> 150
```

### 30.2.18 Date.prototype.setMilliseconds

Date 객체에 밀리초(0 ~999)를 나타내는 정수를 설정

```js
const today = new Date();

// 밀리초 지정
today.setMilliseconds(123);
today.getMilliseconds(); // -> 123
```

### 30.2.19 Date.prototype.getTime

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환

```js
new Date("2020/07/24/12:30").getTime(); // -> 1595561400000
```

### 30.2.20 Date.prototype.setTime

Date 객체에 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 설정

```js
const today = new Date();

today.setTime(86400000); // 1day
console.log(today); // -> Fri Jan 02 1970 09:00:00 GMT+0900 (Korean Standard Time)
```

### 30.2.21 Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환

```js
const today = new Date(); // today의 지정 로캘은 KST

// UTC와 today의 지정 로캘 KST와의 차이는 -9시간
today.getTimezoneOffset() / 60; // -9
```

### 30.2.22 Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환

```js
const today = new Date("2020/07/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (Korean Standard Time)
today.toDateString(); // -> Fri Jul 24 2020
```

### 30.2.23 Date.prototype.toTimeString

사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환

```js
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (Korean Standard Time)
today.toTimeString(); // -> 12:30:00 GMT+0900 (Korean Standard Time)
```

### 30.2.24 Date.prototype.toISOString

ISO 8601[🔗](https://ko.wikipedia.org/wiki/ISO_8601) 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환

```js
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (Korean Standard Time)
today.toISOString(); // -> 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // -> 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ""); // -> 20200724
```

### 30.2.25 Date.prototype.toLocaleString

인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환  
인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용

```js
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (Korean Standard Time)
today.toLocaleString(); // -> 7/24/2020, 12:30:00 PM
today.toLocaleString("ko-KR"); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString("en-US"); // -> 7/24/2020, 12:30:00 PM
today.toLocaleString("ja-JP"); // -> 2020/7/24 12:30:00
```

### 30.2.26 Date.prototype.toLocaleTimeString

인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환  
인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용

```js
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (Korean Standard Time)
today.toLocaleTimeString(); // -> 12:30:00 PM
today.toLocaleTimeString("ko-KR"); // -> 오후 12:30:00
today.toLocaleTimeString("en-US"); // -> 12:30:00 PM
today.toLocaleTimeString("ja-JP"); // -> 12:30:00
```

## 30.3 Date를 활용한 시계 예제

현재 날짜와 시간을 초 단위로 반복 출력하는 예제(p.576)

# 31장. RegExp

## 31.1 정규 표현식이란?

정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어  
정규 표현식은 자바스크립트의 고유 문법이 아니며 대부분 프로그래밍 언어와 코드 에디터에 내장  
자바스크립트는 펄(Perl)의 정규 표현식 문법을 ES3부터 도입

정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공  
**패턴 매칭 기능**: 특정 패턴과 일치하는 문자열을 검색, 추출 또는 치환할 수 있는 기능

정규표현식의 장단점

- 장점: 반복문과 조건문 없이 패턴을 정의, 테스트하는 것으로 간단히 확인 가능
- 단점: 주석, 공백을 허용하지 않고 여러 가지 기호를 혼잡하여 사용하여 가독성이 좋지 않음

## 31.2 정규 표현식의 생성

정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수 사용 가능

- 정규 표현식 리터럴을 사용하여 정규 표현식 객체 생성

  ```js
  const target = "Is this all there is?";

  // 패턴: is
  // 플래그: i => 대소문자를 구별하지 않고 검색
  const regexp = /is/i;

  // test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색,
  // 매칭 결과를 불리언 값으로 반환
  regexp.test(target); // -> true
  ```

- RegExp 생성자 함수를 사용하여 RegExp 객체 생성

  ```js
  const target = "Is this all there is?";

  const regexp = new RegExp(/is/i); // ES6

  regexp.test(target); // -> true
  ```

## 31.3 RegExp 메서드

### 31.3.1 RegExp.prototype.exec

exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색, 매칭 결과를 배열로 반환  
매칭 결과가 없는 경우 null 반환

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target);
// -> ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환

### 31.3.2 RegExp.prototype.test

test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색, 매칭 결과를 불리언 값으로 반환

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // -> true
```

### 31.3.3 String.prototype.match

String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환

```js
const target = "Is this all there is?";
const regExp = /is/;

target.match(regExp);
// -> ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

exec 메서드: g 플래그 지정해도 첫 번째 매칭 결과만 반환  
String.prototype.match 메서드: g 플래그가 지정되면 모든 매칭결과를 배열로 반환

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // -> ['is', 'is']
```

## 31.4 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용  
플래그 6개 존재, 중요한 3개의 플래그

| 플래그 | 의미        | 설명                                                       |
| :----- | :---------- | :--------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속                  |

플래그는 옵션이므로 선택적 사용 가능  
순서와 상관없이 하나 이상의 플래그 동시에 설정 가능

## 31.5 패턴

정규 표현식은 "일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어"

정규 표현식의 구성

- 패턴: 문자열의 일정한 규칙을 표현하기 위해 사용
- 플래그: 정규 표현식의 검색 방식을 설정하기 위해 사용

패턴은 \/로 열고 닫으며 문자열의 따옴표는 생략

### 31.5.1 문자열 검색

정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열 검색

```js
const target = "Is this all there is?";

const regExp = /is/;

regExp.test(target); // -> true
target.match(regExp);
// -> ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

### 31.5.2 임의의 문자열 검색

. 은 임의의 문자 한 개를 의미

```js
const target = "Is this all there is?";

const regExp = /.../g; // 임의의 3자리 문자열, 대소문자 구별, 전역 검색

target.match(regExp); // -> ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

### 31.5.3 반복 검색

{m, n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미  
콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의

- {m,n}

  ```js
  const target = "A AA B BB Aa Bb AAA";

  const regExp = /A{1,2}/g; // 'A'가 최소 1번, 최대 2번 반복되는 문자열, 전역 검색

  target.match(regExp); // -> ['A', 'AA', 'A', 'AA', 'A']
  ```

{n}은 앞선 패턴이 n번 반복되는 문자열을 의미({n}은 {n,n}과 동일)

- {n}

  ```js
  const target = "A AA B BB Aa Bb AAA";

  const regExp = /A{2}/g; // 'A'가 2번 반복되는 문자열, 전역 검색

  target.match(regExp); // -> ['AA', 'AA']
  ```

{n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미

- {n,}

  ```js
  const target = "A AA B BB Aa Bb AAA";

  const regExp = /A{2,}/g; // 'A'가 최소 2번 이상 반복되는 문자열, 전역 검색

  target.match(regExp); // -> ['AA', 'AAA']
  ```

+는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미(+는 {1,}과 동일)

- \+

  ```js
  const target = "A AA B BB Aa Bb AAA";

  const regExp = /A+/g; // 'A'가 최소 한 번 이상 반복되는 문자열, 전역 검색

  target.match(regExp); // -> ['A', 'AA', 'A', 'AAA']
  ```

?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미(?는 {0,1}과 동일)

- ?

  ```js
  const target = "color colour";

  // 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복
  // 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색
  const regExp = /colou?r/g;

  target.match(regExp); // ->  ['color', 'colour']
  ```

### 31.5.4 OR 검색

|는 or의 의미

```js
const target = "A AA B BB Aa Bb";

const regExp = /A|B/g; // 'A' 또는 'B'를 전역 검색

target.match(regExp); // -> ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

활용

- 앞선 패턴을 한 번 이상 반복

  ```js
  const target = "A AA B BB Aa Bb";

  // 'A' 또는 'B'가 한 번 이상 반복되는 문자열, 전역 검색
  const regExp = /[AB]+/g;

  target.match(regExp); // -> ['A', 'AA', 'B', 'BB', 'A', 'B']
  ```

- 범위 지정

  ```js
  const target = "A AA BB ZZ Aa Bb";

  // 'A' ~ 'Z'가 한 번 이상 반복되는 문자열, 전역 검색
  const regExp = /[A-Z]+/g;

  target.match(regExp); // -> ['A', 'AA', 'BB', 'ZZ', 'A', 'B']
  ```

### 31.5.5 NOT 검색

\[ ... ] 안의 ^는 not의 의미

```js
const target = "AA BB 12 Aa Bb";

const regExp = /[^0-9]+/g; // 숫자를 제외한 문자열, 전역 검색

target.match(regExp); // -> ['AA BB ', ' Aa Bb']
```

### 31.5.6 시작 위치로 검색

\[ ... ] 밖의 ^는 문자열의 시작을 의미

```js
const target = "https://poiemaweb.com";

const regExp = /^https/; // 'https'로 시작하는지 검사

regExp.test(target); // -> true
```

### 31.5.7 마지막 위치로 검색

$는 문자열의 마지막을 의미

```js
const target = "https://poiemaweb.com";

const regExp = /com$/; // 'com'로 끝나는지 검사

regExp.test(target); // -> true
```

## 31.6 자주 사용하는 정규표현식

### 31.6.1 특정 단어로 시작하는지 검사

```js
const url = "https://example.com";

// 'http://' 또는 'https://'로 시작하는지 검사
/^https?:\/\//.test(url); // -> true
/^(http|https):\/\//.test(url); // -> true
```

### 31.6.2 특정 단어로 끝나는지 검사

```js
const fileName = "index.html";

// 'html'로 끝나는지 검사
/html$/.test(fileName); // -> true
```

### 31.6.3 숫자로만 이루어진 문자열인지 검사

```js
const target = "12345";

// 숫자로만 이루어진 문자열인지 검사
/^\d+$/.test(target); // -> true
```

\\d : 숫자를 의미

### 31.6.4 하나 이상의 공백으로 시작하는지 검사

```js
const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사
/^[\s]+/.test(target); // -> true
```

\\s: 여러 가지 공백 문자를 의미(\\s는 \[\\t\\r\\n\\v\\f]와 동일)

### 31.6.5 아이디로 사용 가능한지 검사

```js
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

### 31.6.6 메일 주소 형식에 맞는지 검사

```js
const email = "ungmo2@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  email
); // -> true
```

### 31.6.7 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

### 31.6.8 특수 문자 포함 여부 검사

```js
const target = "abc#123";

// A-Za-z0-9 이외의 문자가 있는지 검사
/[^A-Za-z0-9]/gi.test(target); // -> true
```
