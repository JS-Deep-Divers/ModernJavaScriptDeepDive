### 32. String

- 문자열은 원시값이므로 변경할 수 없다.

- 변경하려고 해도 에러는 발생하지 않는다. (변하지 않을뿐)

# String.prototype.indexOf

- 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.

- 검색에 실패하면 -1을 반환한다.

# String.prototype.search

- 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.

- 검색에 실패하면 -1을 반환한다.

# String.prototype.includes

- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 결과를 true 또는 false로 반환한다.

# String.prototype.startsWith

- 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 true 또는 false로 반환한다.

# String.prototype.endsWith

- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true 또는 false로 반환한다.

# String.prototype.charAt

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.

# String.prototype.substirng

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전까지의 부분 문자열을 반환한다.

# String.prototype.slice

- substring 메서드와 동일하게 동작한다.

- 음수인 인수를 전달하면 대상의 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

# String.prototype.toUpperCase

- 대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

# String.prototype.toLowerCase

- 대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

# String.prototype.trim

- 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

# String.prototype.repeat

- 대상 문자열을 인수로 전달받은 정수만큼 반복해 새로운 문자열을 반환한다.

- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError을 발생시킨다.

- 인수를 생략하면 기본값 0이 설정된다.

# String.prototype.replace

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규 표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

# String.prototype.split

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.

- 인수로 빈 문자열`('')`을 전달하면 각 문자를 모두 분리하고, 인수를 생략`()`하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

### 33. 7번쨰 데이터 타입 Symbol

## 심벌이란?

- 변경 불가능한 원시 타입의 값

- 다른 값과 중복되지 않는 유일무이한 값

- 주로 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

```js
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.

const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

# Symbol.for / Symbol.keyfor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.

- 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```js
// 전역 심벌 레지스트리에 mSymbol 이라는 키로 저장된 심벌 값이 없으며 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

- Symbol 함수는 호출될때 마다 유일무이한 심벌 값을 생성한다.

- Symbol.keyfor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 킬르 추출할 수 있다.
