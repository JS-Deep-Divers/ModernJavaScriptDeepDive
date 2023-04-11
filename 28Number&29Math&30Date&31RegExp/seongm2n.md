### 28

- Number : 표준 빌트인 객체

[28.1] Number 생성자 함수

```jsx
const numobj = new Number();
console.log(numObj); //Number {[[PrimitiveValue]]:0}
```

- Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성
- Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후, [[NumberData]]내부 슬롯에 변환된 숫자를 할당한 Number래퍼 객체를 생성
- new 연산자를 사용하지 않고 Number생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환 → 이를 이용하여 명시적으로 타입을 변환

[28.3] Number 메서드

(1) Number.isFinite

- 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환

(2) Number.isInteger

- 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환
- 검사 전에 인수를 숫자로 암묵적 타입 변환하지 않음

(3) Number.isNaN

- 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 변환

(4) Number.isSafeInteger

- 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 변환

(5) Number.prototype.toExponential

- 숫자를 지수 표기법으로 변환하여 문자열로 반환
    - 지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 주로 사용

(6) Number.prototype.toFixed

- 숫자를 반올림하여 문자열로 반환

```jsx
//소수점 이하 반올림. 인수를 생략하면 기본값 0이 지정
(12345.6789).toFixed(); //"123456"
//소수점 이하 1자릿수 유효, 나머지 반올림
(12345.6789).toFixed(1); // "12345.7
//소수점 이하 2자릿수 유효, 나머지 반올림
(12345.6789).toFixed(2); // "12345.68"
```

(7) Number.prototype.toPrecision

- toPrecision메서드는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환

(8) Number.prototype.toString

- 숫자를 문자열로 변환
- 진법을 나타내는 2~36사이의 정수값을 인수로 전달
- 인수를 생략하면 기본값 10진법 지정

```jsx
(10).toString(); //"10"
(16).toStirng(2); //"10000"
(16).toString(8); //"20"
(16).toString(16); //"10"
```

### 29

[29.1] Math 프로퍼티

(1) Math.PI

- 원주율 PI값 반환

```jsx
Math.PI; // 3.14159265389793
```

[29.2] Math 메서드

(1) Math.abs

- 인수로 전달된 숫자의 절대값을 반환
- 절대값은 반드시 0 또는 양수

(2) Math.round

- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환

(2) Math.ceil

- 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환

(3) Math.floor

- 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환

(4) Math.sqrt

- 인수로 전달된 숫자의 제곱근 반환

(5) Math.random

- 임의의 난수(랜덤 숫자)를 반환
- 0~1 미만의 실수, 0은 포함이지만 1은 포함되지 않음

(6) Math.pow

- 첫번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과 반환

(7) Math.max

- 전달받은 인수 중 가장 큰 수 반환

(8) Math.min

- 전달받은 인수 중에서 가장 작은 수 반환

### 30

- 표준 빌트인 객체인 Date는 날짜와 시간(연,월,일,시,분,초,밀리초,천분의1초)을 위한 메서드 제공하는 빌트인 객체이면서 생성자 함수

[30.1] Date 생성자 함수

(1) new Date()

- 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 Date객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력
- new연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열 반환

(2) new Date(milliseconds)

- 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date객체를 반환

(3) new Date(dateString)

- 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date객체를 반환
- 이때 인수로 전달한 문자열은 Date.parse메서드에 의해 해석 가능한 형식이어야 함

(4)new Date(year,month[,day,hour,minute,second,millisecond])

[30.2] Date 메서드

(1) Date.now

- 현재시간까지 경과한 밀리초를 숫자로 반환

(2) Date.parse

- 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환

(3) Date.UTC

- new Date(year,month[,day,hour,minute,second,millisecond])와 같은 형식의 인수 사용

(4) Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수 반환

(5) Date.prototype.setFullYear

- Date객체에 연도를 나타내는 정수 설정, 연도 이외에 옵션으로 월, 일 설절

(6) Date.prototype.getMonth

- Date 객체의 월을 나타내는 0~11의 정수 반환, 1월은 0, 12월은 11

(7) Date.prototype.setMonth

- Date 객체의 월을 나타내는 0~11의 정수 설정

(8) Date.prototype.getDate

- Date 객체의 날짜(1~31)를 나타내는 정수 반환

(9) Date.prototype.setDate

- Date 객체의 날짜(1~31)를 나타내는 정수 설정

(10) Date.prototype.getDay

- Date 객체의 요일(0~6)을 나타내는 정수 반환

(11) Date.prototype.getHours

- Date 객체의 시간(0~23)을 나타내는 정수 반환

(12) Date.prototype.setHours

- Date 객체의 시간(0~23)을 나타내는 정수 설정

(13) Date.prototype.getMinutes

- Date 객체의 분(0~59)을 나타내는 정수 반환

(14) Date.prototype.setMinutes

- Date 객체의 분(0~59)을 나타내는 정수 설정

(15) Date.prototype.getSeconds

- Date 객체의 초(0~59)을 나타내는 정수 반환

(16) Date.prototype.setSeconds

- Date 객체의 분(0~59)을 나타내는 정수 설정

(17) Date.prototype.getMilleseconds

- Date 객체의 밀리초(0~999)을 나타내는 정수 반환

(18) Date.prototype.setMilliseconds

- Date 객체의 밀리초(0~999)을 나타내는 정수 설정

(19) Date.prototype.getTime

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date객체의 시간까지 경과된 밀리초 반환

(20) Date.prototype.setTime

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date객체의 시간까지 경과된 밀리초 설정

(21) Date.prototype.getTimezoneOffset

- UTC와 Date 객체에 지정된 로캘 시간과의 차이를 분 단위로 반환

(22) Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date객체의 날짜를 반환

(23) Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 Date객체의 시간을 표현한 문자열을 반환

(24) Date.prototype.toISOString

- ISO 8601형식으로 Date객체의 날짜와 시간을 표현한 문자열을 반환

(25) Date.prototype.toLocalString

- 인수로 전달된 로캘을 기준으로 Date객체의 날짜와 시간을 표현한 문자열 반환

(26) Date.prototype.toLocalTimeString

- 인수로 전달된 로캘을 기준으로 Date객체의 시간을 표현한 문자열 반환

### 31

[31.1] 정규 표현식이란?

<aside>
💡 /regexp/i

/→시작 regexp→패턴 /→종료 i→플래그

</aside>

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)
- 자바스크립트 고유 문법은 아님
- 패턴 매칭 기능 제공
    - 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능
- 정규표현식을 사용하지 않는다면 반복문과 조건문을 ‘첫 번째 문자가 숫자이고 이어지는 문자도 숫자이고 이어지는 문자도 숫자이고 다음은’-’이고 …’와 같이 한 문자씩 연속해서 체크
- 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다만
- 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않은 문제가 있음

[31.2] 정규 표현식의 생성

- 정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp생성자 함수를 사용
- 일반적인 방법은 정규 표현식 리터럴을 사용

```jsx
const target = 'Is this all there is?';

const regexp = /is/i;
regexp.text(target); //true
```

[31.3] RegExp 메서드

(1) RegExp.prototype.exec

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
- 매칭 결과가 없는 경우 null반환

(2) RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규 표현식이 패턴을 검색하여 매칭 결과를 불리언 값으로 반환

(3) String.prototype.match

- String 표준 빌트인 객체가 제공하는 match메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭결과르 배열로 반환

[31.4] 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용


    | 플래그 | 의미 | 설명 |
    | --- | --- | --- |
    | i | Ignore case | 대소문자를 구별하지 않고 패턴을 검색 |
    | g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
    | m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색을 계속함 |
- 플래그는 옵션이므로 선택적으로 사용
- 순서와 상관없이 하나 이상의 플래그를 동시에 설정

[31.5] 패턴

- “일정한 규칙을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어”
- 정규 표현식은 패턴 + 플래그로 구성
