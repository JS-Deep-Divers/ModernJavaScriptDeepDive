# 24장. 클로저

클로저(closure)는 자바스크립트 고유의 개념이 아님  
함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성

> "A closure is the the combination of a function and the lexical environment within which that function was declared."
> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
> \- MDN [🔗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

이해해야 할 핵심 키워드 "함수가 선언된 렉시컬 환경"

```js
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x); // 10
  }

  innerFunc();
}

outerFunc();
```

outerFunc 함수 내부에서 중첩 함수 innerFunc가 정의되고 호출  
이때 중첩함수 innerFunc의 상위 스코프는 외부 함수 outerFunc의 스코프  
따라서 중첩 함수 innerFunc 내부에서 자신을 포함하고 있는 외부 함수 outerFunc의 x 변수에 접근 가능

```js
const x = 1;

function outerFunc() {
  const x = 10;
  innerFunc();
}

function innerFunc() {
  console.log(x); // 1
}

outerFunc();
```

만약 innerFunc 함수가 outerFunc 함수의 내부에서 정의된 중첩 함수가 아니라면 innerFunc 함수를 outerFunc 함수의 내부에서 호출 하더라도 outerFunc 함수의 변수에 접근 불가  
(자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문)

## 24.1 렉시컬 스코프

**렉시컬 스코프(정적 스코프)**  
자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정

```js
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

foo, bar 함수는 모두 전역에서 정의된 전역 함수  
함수의 상위 스코프는 함수를 어디서 정의했느냐에 따라 결정되므로 foo, bar 함수의 상위 스코프는 전역  
함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못함  
함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정 및 불변

스코프의 실체 &rarr; 실행 컨텍스트의 렉시컬 환경  
**스코프체인**  
렉시컬 환경은 자신의 "외부 렉시컬 환경에 대한 참조"를 통해 상위 렉시컬 환경과 연결

따라서 "함수의 상위 스코프를 결정한다"는 것은 "렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다"는 것과 동일  
렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값  
&rarr; 상위 렉시컬 환경에 대한 참조(상위 스코프)

**렉시컬 스코프**  
렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값(상위 스코프에 대한 참조)은 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정

## 24.2 함수 객체의 내부 슬롯 \[\[Environment]]

렉시컬 스코프가 가능하려면 함수는 자신이 정의된 환경(상위 스코프: 함수 정의가 위치하는 스코프) 기억 필요  
이를 위해 함수는 자신의 내부 슬롯\[\[Environmet]]에 자신이 정의된 환경(상위 스코프의 참조)를 저장

```js
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정
  // 함수 호출 위치와 상위 스코프는 관계 없음
  bar();
}

// 함수 bar는 자신의 상위 스코프(전역 레시컬 환경)을 [[Environmet]]에 저장
function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

## 24.3 클로저와 렉시컬 환경

```js
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // ②
  return inner;
}

// outer 함수 호출 시, 중첩 함수 inner 반환
// outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

outer 함수 호출(③)하면 outer 함수는 중첩 함수 inner를 반환하고 생명 주기 마감  
(outer 함수의 실행이 종료 &rarr; outer 함수의 실행 컨텍스트, 실행 컨텍스트 스택에서 제거)  
outer 함수의 지역 변수 x 또한 생명 주기 마감(outer 함수의 실행 컨텍스트가 제거되었으므로)

하지만 실행 결과 outer 함수의 지역 변수 x의 값인 10  
이미 생명 주기가 종료되어 제거된 outer 함수의 지역 변수가 동작

**클로저**
외부 함수보다 중첩 함수가 더 오래 유지되는 경우, 이미 생명 주기가 종료한 외부 함수의 변수를 참조하는 중첩 함수  
클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적  
클로저에 의해 참조되는 상위 스코프의 변수를 **자유 변수**라고 부름  
클로저란 "함수가 자유 변수에 대해 닫혀있다"라는 의미

## 24.4 클로저의 활용

클로저는 상태를 안전하게 변경하고 유지하기 위해 사용  
즉, 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉, 특정 함수에게만 상태 변경을 허용하기 위해 사용

```js
let num = 0; // 카운트 상태 변수

// 카운트 상태 변경 함수
const increase = function () {
  return ++num; // 카운트 상태를 1만큼 증가
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

잘 동작하지만 오류 발생 가능성을 내포하고 있는 좋지 않은 코드

이유

1. 카운트 상태(num)는 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 함
2. 이를 위해 카운트 상태(num)는 increase 함수만이 변경할 수 있어야 함

카운트 상태(num)는 누구나 접근, 변경 가능(암묵적 결합)  
&rarr; 의도치 않게 상태가 변경될 수 있다는 의미

increase 함수만이 num 변수를 참조하고 변경할 수 있게 지역 변수로 작성

```js
// 카운트 상태 변경 함수
const increase = function () {
  let num = 0; // 카운트 상태 변수

  return ++num; // 카운트 상태를 1만큼 증가
};

// 이전 상태를 유지 못함
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

전역 변수 num을 increase 함수의 지역 변수로 변경하여 의도치 않은 상태 변경은 방지했으나,  
increase 함수가 호출될 때마다 num은 선언 및 0으로 초기화를 반복하여 언제나 1  
상태가 변경되기 이전 상태를 유지 못함

이전 상태 유지를 위해 클로저를 사용

```js
// 카운트 상태 변경 함수
const increase = (function () {
  let num = 0; // 카운트 상태 변수

  // 클로저
  return function () {
    return ++num; // 카운트 상태를 1만큼 증가
  };
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

코드가 실행되면 즉시 실행 함수 호출, 즉시 실행 함수가 반환한 함수가 increase 변수에 할당  
반환된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저  
즉시 실행 함수가 반환한 클로저는 num을 언제 어디서 호출하든지 참조하고 변경 가능  
즉시 실행 함수는 한 번만 실행되므로 num 변수가 초기화되지 않음  
num 변수는 외부에서 직접 접근할 수 없는 은닉된 private 변수

클로저

- 상태가 의도치 않게 변경되지 않도록 안전하게 은닉
- 특정 함수에게만 상태 변경을 허용하여 안전하게 변경하고 유지

## 24.5 캡슐화와 정보 은닉

**캡슐화**  
객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것

**정보 은닉**  
캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용  
이는 구현의 일부를 감추어 적절치 못한 접근으로부터 객체의 상태 변경을 방지 및 정보 보호  
객체 간의 상호 의존성(결합도)를 찾추는 효과

자바스크립트는 public, private, protected 같은 접근 제한자를 제공하지 않음  
자바스크립트 객체의 모든 프로퍼티와 메서드는 기본적으로 외부에 공개(기본적으로 public)

```js
function Person(name, age) {
  this.name = name; // public
  let _age = age; // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age_); // undefined
```

해당 코드의 name 프로퍼티는 현재 외부로 공개되어 있어 자유롭게 참조 및 변경 가능  
name 프로퍼티는 public, \_age 변수는 private

자바스크립트는 정보 은닉을 완전하게 지원하지 않음

## 24.6 자주 발생하는 실수

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```

① 에서 함수가 funcs 배열의 요소로 추가  
② 에서 funcs 배열의 요소로 추가된 함수를 순차적으로 호출
출력 결과는 0 1 2 가 아닌 3 3 3 (i 변수는 함수 레벨 스코프, 전역 변수)

클로저를 사용하여 수정

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = (function (id) {
    // ①
    return function () {
      return id;
    };
  })(i);
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

① 에서 즉시 실행 함수는 전역 변수 i에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료  
즉시 실행 함수가 반환한 함수는 funcs 배열에 순차적으로 저장  
즉시 실행 함수의 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수의 상위 스코프에 존재  
즉시 실행 함수가 반환한 중첩 함수는 자신의 상위 스코프를 기억하는 클로저, 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 해당 값 유지

이 현상은 ES6의 let 키워드 또는 함수형 프로그래밍 기법인 고차 함수를 사용하는 방법으로 해결 가능
