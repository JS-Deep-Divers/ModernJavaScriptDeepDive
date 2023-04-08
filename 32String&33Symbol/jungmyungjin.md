# 2023.04.03 자바스크립트 딥다이브 (32.String/33.Symbol)

## 기억하고 싶은 내용

### String

- **문자열은 원시 값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다**

```javascript
const strObj = new String('Lee');
console.log(strObj); // String{'Lee'} -> string 객체 리턴

console.log(strObj[0]) // L
strObj[0] 'S';
console.log(strObj); // String{'Lee'}
```

- String 객체의 메서드는 언제나 새로운 문자열을 반환한다.
  - 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.

### String 메서드

**startWith**

- 대상 문자열이 인수로 전달받은 문자욜로 시작하는지 확인하여 그 결과를 `true`/`false`로 반환한다.

**endWith**

- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를  `true`/`false`로 반환한다.

**slice**

- **slice**는 `substring`메서드와 동일하게 동작한다. <u>단, slice 메서드에는 음수인 인수를 전달할 수 있다</u>

```javascript
const str = "hello world";
str.slice(-5); // -> 'world'
```



### Symbol

- 심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입이다.
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.
- 주로 이름의 충돌 위험 없는 유일한 프로퍼티 키를 만들기 위해 사용한다



**symbol 생성**

- 다른 원시값 생성때 처럼 리터럴 표기법을 통해 생성할 수 없다.
- 심벌 값은 **Symbol 함수를 호출하여 생성해야 한다**.
- 심벌 값은 생성자 함수로 객체를 생성하는 것처럼 보이지만 생성자 함수와는 달리 <u>`new` 연산자와 함께 호출하지 않는다</u>



**symbol 메서드** : `Symbol.for` /`Symbol.keyFor`

- 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다



### 심벌과 프로퍼티

- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.

- 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다

  ```javascript
  const obj = {
    [Symbol.for('mySymbol')]: 1
  };
  obj[Symbol.for('mySymbol')]; // -> 1
  ```



### Well-kown Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 **Well-known Symbol** 이라 부른다.
- `Well-kown- Symbol`은 자바스크립트 엔진의 내부 알고리즘에 사용된다.







## 느낀점

- string 부분을 읽을 때, 이미 사용하고 있는 메소드나 내용들이 많아 읽기 수월하였다.
- string은 원시값이므로 값을 변경할 수 없는 부분은 실수할 수 있는 부분이라 주의해야겠다. ~~심지어 오류도 안나오다니...~~
- Symbol은 사실 익숙하지 않은 데이터 타입이다.~~충돌할 위험이 있을만한 키를 갖는 크기의 프로젝트를 하지 않아서일까..~~
- 그래도 딥다이브 책을 읽은 덕분에 어떠한 것들을 할 수 있는지 어렴풋이 알게된 것 같다.