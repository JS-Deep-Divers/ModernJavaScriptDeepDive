# 12장. 함수

## Q1.

다음의 명칭을 적으시오.

```js
function f(x) {
  return x;
}

f(10);
```

\_\_①\_\_: 함수 내부로 입력을 전달받는 변수, x

\_\_②\_\_: 함수 호출시 입력값, 10

함수를 실행하기 위해 필요한 값을 함수 외부에서 함수 내부로 전달할 필요가 있는 경우, \_\_①\_\_를 통해 \_\_②\_\_를 전달한다. \_\_②\_\_는 함수를 호출할 때 지정한다. \_\_①\_\_는 함수를 정의할 때 선언하며, 함수 몸체 내부에서 변수와 동일하게 취급된다.

## A1.

① 매개변수(parameter)

② 인수(argument)

p155, 168

## Q2.

다음 함수 선언문을 화살표 함수로 정의하시오.

```js
function add(x, y) {
  return x + y;
}
```

## A2.

```jsx
var add = (x, y) => x + y;
```

p.158, 167

## Q3.

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수는?

```js
// ____ 함수를 사용하는 고차 함수 map
var res = [1, 2, 3].map(function (item) {
  return item * 2;
});

// ____ 함수를 사용하는 고차 함수 filter
res = [1, 2, 3].filter(function (item) {
  return item % 2;
});
```

## A3.

콜백 함수

p.183 ~ 186

## Q4.

다음 빈칸에 알맞은 용어는?

```js
// 함수 참조
console.log(add); // f add(x, y)
console.log(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 \_\_\_\_이라 한다.

## A4.

함수 호이스팅

p.164 ~ 165
