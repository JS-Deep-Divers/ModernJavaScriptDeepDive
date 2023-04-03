### 8장 제어문

- 조건에 따라 코드 블록을 실행하거나 반복 실행 할 때 사용
- 일반적을 코드는 위에서 아래 방향으로 순차적으로 실행되지만, 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있음

## 블록문

- 블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 함
- 자바스크립트에서는 블록문을 하나의 실행 단위로 취급
- 블록문의 끝에는 세미콜론(;)을 붙이지 않음

## while문

- 조건문의 평가 결과가 언제나 참이면 무한루프가 된다.
- 무한루프에서 탈출하기 위해서는 코드블록 내에 if문으로 탈출 조건을 만들고 break문으로 코드 블록을 탈출한다.

# do...while문

- 코드 블록을 먼저 실행하고 조건식을 평가한다.
- 코드 블록은 무조건 한 번 이상 실행된다.

# continue문

- 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이해시킨다.
- break 처럼 반복문을 탈출하지는 않는다.

### 9장 타입 변환과 단축 평가

** 명시적 타입 변환 : 개발자가 의도적으로 타입을 변환하는 것
** 암묵적 타입 변환 : 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동으로 변환하는 것

## 숫자 타입으로 변환

```js
//문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  //불리언 타입
  true + // -> 1
  false + // -> 0
  //null 타입
  null + // -> 0
  //undefined 타입
  undefined + // -> NaN
  //심벌 타입
  Symbol() + // -> TypeError
  //객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

## 단축 평가

- 논리합(||) 또는 논리곱(&&) 연산자 표현식은 어느 한 쪽으로 평가된다.

```js
//논리합(||) 연산자
"Cat" || "Dog"; // -> 'Cat'
"false" || "Dog"; // -> 'Dog'
"Cat" || "false"; // -> 'Cat'

"Cat" && "Dog"; // -> 'Dog'
"false" && "Dog"; // -> 'false'
"Cat" && "false"; // -> 'false'
```

## 옵셔널 체이닝 연산자

- 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.