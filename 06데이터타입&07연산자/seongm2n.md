### 6장

[템플릿 리터럴]

- 문자열 표기법
- 백틱사용(``)
- 문자열 연산자보다 가독성 좋고 간편하게 문자열 조합

<예시>

```jsx
var first = "Seongmin"
var last = "Shin"

//표현식 삽입
console.log(`My name is ${first} ${last}.`);
//My name is Seongmin Shin.
```

- 표현식 삽입하려면 ${}으로 표현식을 감쌈
- 표현식의 평가 결과가 문자열이 아니더라도 문자열로 타입이 강제로 변환

[객체 타입]

- JS를 이루고 있는 모든 것이 객체

### 7장

[삼항 조건 연산자]

- 조건식 ? 조건식이 true일 때 반환할 값: 조건식이 false일 때 반환할 값

[지수 연산자]

- 지수 연산자가 도입되기 이전에는 Math.pow메서드 사용
    - `Math.pow(2,2) //4`
- 지수 연산자가 도입한 후 가독성이 좋다.
    - `2**2 //4`
- 지수 연산자는 이항 연산자 중에서 우선순위가 높음
