### 28. Number

### 29. Math

### 30. Date

## new Date();

- Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

- Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.

- Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.

### regExp

## 정규 표현식이란?

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.

- 자바스크립트 고유 문법이 아니며, 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.

- 정규 표현식은 문자열을 대상으로 `패턴 매칭 기능`을 제공한다.

```js

// 사용자로부터 입력받은 휴대폰 전화번호
const tek=l = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // false

```

## 정규 표현식의 생성

```js
const target = "Is this all there is?";

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를
// 불리언 값으로 반환한다.
regexp.test(target); // true
```

- RegExp 생성자 함수를 사용하여 regExp 객체를 생성할 수도 있다

```js

// pattern: 정규 표현식의 패턴
// flags: 정규 표현식의 플래그(g, i, m, u, y)

new RegExp(pattern[, flags])

```

## RegExp 메서드

**RegExp.prototype.exec**
=> exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 반환한다.
=> 매칭 결과가 없는 경우 null을 반환한다.

**RegExp.prototype.test**
=> test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

**RegExp.prototype.match**
=> String 표준 빌트인 객체가 제공하는 match 메거드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.
=> exec 메서드는 g 플래그를 지정해도 첫 번쨰 결과만 반환하지만, match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // ["is", "is"]
```

## 플래그

- i : 대소문자를 구별하지 않고 패턴을 검색한다.
- g : 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
- m : 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.
