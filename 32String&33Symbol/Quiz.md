# 32. String / 33. 7번째 데이터 타입 Symbol

# Q1.

다음 예제 코드의 실행 결과를 적으시오.

```js
const str = "Hello World";

console.log(str.slice(-5), str.substring(-5));
```

## A1.

World Hello World

p.599 ~ 600

# Q2.

다음 빈칸에 알맞은 용어를 채우시오.
심벌은 (A)에서 도입된 7번째 데이터 타입으로 변경 불가능한 (B) 타입의 값이다. 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.

A:
B:

## A2.

A: ES6
B: 원시

p.605

# Q3.

다음 예제 코드의 실행 결과를 적으시오.

```js
const obj = {
  [Symbol("mySymbol")]: 1,
};

console.log(Object.keys(obj));
```

## A3.

[]

p.610
