### 32

문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공

[32.3] String 메서드

(1) String.prototype.indexOf

- 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환

(2) String.prototype.search

- 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환

```jsx
const str = 'Hello world';

//정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환
srt.search(/o/); // -> 4
str.search(/x/); // -> -1
```

(3) String.prototype.includes

- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 true 또는 false로 반환

(13) String.prototype.replace

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두번째 인수로 전달한 문자열로 치환한 문자열을 반환

(14) String.prototype.split

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환

```jsx
const str = 'How are you doing?';

// \s는 여러 가지 공백 문자(스페이스,탭 등)을 의미
// 즉, [\t\r\n\v\f]와 같은 의미
str.split(/\s/); //->["How","are","you","doing?"]
```

### 33

(1) Symbol 함수

- 심벌 값은 Symbol함수를 호출하여 생성
- 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, **다른 값과 절대 중복되지 않는 유일무이한 값**

```jsx
const mySymbol = Symbol();

//심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않음
console.log(mySymbol + ''); //typeError: cannot convert a Symbol value to a string
console.log(+mySymbol); //typeError: cannot convert a Symbol value to a number
```

- 단, 불리언 타입으로는 암묵적으로 타입 변환
- if문 등에서 존재 확인 가능

```jsx
const mySymbol = Symbol();

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); //true
// if문 등에서 존재 확인 가능
if(mySymbol) console.log('mySymbol is not empty.');
```

[33.7] Well-known Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol이라 부름
- Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용
- Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있음
- 빌트인 이터러블은 이터레이션 프로토콜을 준수
