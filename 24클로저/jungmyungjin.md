# 자바스크립트 딥 다이브_24 클로저

## 기억하고 싶은 내용

> 클로저는 자바스크립트 고유 개념이 아니다.
>
> 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어(예: 하스켈, 리스프, 얼랭, 스칼라 등)에서 사용되는 중요한 특성이다
>
> MD에서는 클로저에 대해 다음과 같이 정의하고 있다.
>
> "A closure is the combination of a function and the lexical environment within which that function was declared." (클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.)



가장 먼저 이해해야 할 개념은 **함수가 선언된 렉시컬 환경** 이다.

```javascript
const x = 1;

function outerFunc(){
  const x = 10;
  innerFunc();
}

function innerFunc(){
  console.log(x); // 1
}

outerFunc();
```

이 같은 현상이 발생하는 이유는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.



### 렉시컬 스코프

자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다.

이를 렉시컬 스코프(정적 스코프)라 한다.



### 함수 객체의 내부슬롯

함수 객체의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프이다.



## 클로저

**외부 함수보다 중첩 함수가 더 오래 유지되는 경우, 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이때 이러한 중첩 함수를 클로저라 부른다.**



```javascript
const x = 1;

function outer(){
  const x = 10;
  const inner = function () { console.log(x)};
  return inner;
}

const innerFunc = outer();
innerFunc();
```

- 이 때 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 **outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.**



### 클로저가 아닌 것들

- 자스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저이다.
- 다만, **상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아니다.**
- 상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 ㅇ낳는다.
- 중첩함수지만, 외부 함수보다 일찍 소멸되는 경우, 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않으므로 클로저라 하지 않는다.



### 자유 변수

- 클로저에 의해 참조되는 상위스코프 변수를 자유변수라고 부른다.
- 클로저란, '함수가 자유 변수에 대해 닫혀있다'라는 의미다. 즉, '자유 변수에 묶여있는 함수' 라고 할 수 있다.



### 활용

외부 상태 변경이나, 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 적극적으로 사용된다.

```javascript
// 이 함수는 카운트 상태를 유지하기 위한 자유변수 count를 기억하는 클로저를 반환한다
const counter = (function (){
  let count = 0;
  
  return function(aux){
    counter = aux(counter);
    return counter;
  };
}());

// 보조함수
function increase(n){
  return ++n;
}
// 보조함수
function decrease(n){
  return --n;
}

// 보조함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0


```







## 느낀점

- 이전에 나왔던 프로토타입, 내부 슬롯, 실행 컨텍스트에 대한 내용들이 연계되어 나오는 것 같다.
- 단순하게 생각하면 쉬운 내용이나, 예외적인 부분이나 예전에 쓰던 사용법들을 생각하면 익숙하지 않다.
- class가 없었을 때의 대체안들이 너무 많아서 오히려 혼란스러운 느낌이다.