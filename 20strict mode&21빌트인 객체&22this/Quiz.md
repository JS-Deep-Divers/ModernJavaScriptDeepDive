# 20. strict mode / 21. 빌트인 객체 / 22. this

## Q1.

다음 예제 코드와 설명을 보고 빈 칸에 알맞은 명칭을 적으시오.

```js
function foo() {
  x = 10;
}
foo();

console.log(x); // 10
```

전역 스코프에도 x 변수 선언이 존재하지 않기 때문에 ReferenceError를 발생시킬 것 같지만 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다. 이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있다. 이러한 현상을 \_\_\_\_\_\_\_ 이라 한다.

## A1.

암묵적 전역(implicit global)

p.313, 339 ~ 341

## Q2.

다음 코드를 보고 출력 결과를 적으시오.

```js
const A = new String("Lee");
console.log(typeof A); // A

const B = new Number(123);
console.log(typeof B); // B
```

A:

B:

## A2.

A: object

B: object

p.321

## Q3.

다음에 알맞은 답을 보기에서 찾아 짝을 지으시오.

| 함수 호출 방식                                             | this 바인딩 |
| ---------------------------------------------------------- | ----------- |
| 일반 함수 호출                                             | A           |
| 메서드 호출                                                | B           |
| 생성자 함수 호출                                           | C           |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | D           |

**보기**

1. Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체
2. 메서드를 호출한 객체
3. 전역 객체
4. 생성자 함수가 (미래에) 생성할 인스턴스

## A3.

A - 3

B - 2

C - 4

D - 1

p.358
