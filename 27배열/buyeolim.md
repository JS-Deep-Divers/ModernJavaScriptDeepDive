# 27장. 배열

## 27.1 배열이란?

배열은 여러 개의 값을 순차적으로 나열한 자료구조

배열 리터럴을 통한 배열 생성

```js
const arr = ["apple", "banana", "orange"];
```

배열은 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성 가능

- **요소**: 배열이 가지고 있는 값  
  &rarr; 자바스크립트의 모든 값은 배열의 요소가 될 수 있음(원시값, 객체, 함수, 배열 등)
- **인덱스**: 배열에서 자신의 위치를 나타내는 0이상의 정수  
  &rarr; 배열의 요소에 접근할 때 사용
  ```js
  arr[0]; // -> 'apple'
  arr[1]; // -> 'banana'
  arr[2]; // -> 'orange'
  ```
- **length 프로퍼티**: 배열 요소의 개수, 배열의 길이를 나타냄
  ```js
  arr.length; // -> 3
  ```
- 배열은 객체 타입
  ```js
  typeof arr; // -> object
  ```
  |      구분       |           객체            |                 배열                  |
  | :-------------: | :-----------------------: | :-----------------------------------: |
  |      구조       | 프로퍼티 키와 프로퍼티 값 | &emsp;&emsp;인덱스와 요소&emsp;&emsp; |
  |    값의 참조    |        프로퍼티 키        |                인덱스                 |
  |    값의 순서    |             X             |                   O                   |
  | length 프로퍼티 |             X             |                   O                   |

## 27.2 자바스크립트 배열은 배열이 아니다

자료구조에서 말하는 배열은 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조  
&rarr; **밀집 배열**: 배열의 요소는 하나의 데이터 타입으로 통일되어 있으며 서로 연속적으로 인접

일반적인 의미의 배열은 인덱스를 통해 단 한 번의 연산으로 임의의 요소에 접근  
&rarr; 임의 접근, 시간복잡도 O(1)  
정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 발견할 때까지 차례대로 검색  
&rarr; 선형 검색, 시간복잡도 O(n)

자바스크립트의 배열은 자료구조에서 말하는 일반적인 의미의 배열과 다름  
&rarr; 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 됨  
&rarr; **희소 배열**: 연속적으로 이어져 있지 않을 수도 있음  
**자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체**

```js
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': {value: 1, writable: true, enumerable: true, configurable: true}
  '1': {value: 2, writable: true, enumerable: true, configurable: true}
  '2': {value: 3, writable: true, enumerable: true, configurable: true}
  length: {value: 3, writable: true, enumerable: false, configurable: false} 
}
*/
```

자바스크립트 배열은 인덱스를 타나내는 문자열을 프로퍼티 키, length 프로퍼티를 갖는 특수한 객체  
자바사크립트 배열의 요소는 프로퍼티 값  
자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 가능

일반적인 배열, 자바스크립트 배열의 장단점

- 일반적인 배열  
  장점: 인덱스로 요소에 빠르게 접근 가능  
  단점: 특정 요소를 검색, 요소를 삽입 또는 삭제하는 경우 효율이 떨어짐
- 자바스크립트 배열  
  장점: 특정 요소 검색, 요소를 삽입 또는 삭제하는 경우 일반적인 배열보다 빠른 성능 기대  
  단점: 해시 테이블로 구현된 객체, 인덱스로 요소에 접근하는 경우 일반적인 배열보다 느림

## 27.3 length 프로퍼티와 희소 배열

length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0 이상의 정수 값($0$ ~ $2^{23} - 1$)  
&rarr; 빈 배열: 0  
&rarr; 빈 배열이 아닐 경우: 가장 큰 인덱스 + 1

```js
console.log([].length); // 0
console.log([1, 2, 3].length); // 3
```

length 프로퍼티의 값은 배열에 요소를 추가, 삭제하면 자동 갱신  
length 프로퍼티의 값보다 큰 숫자 값을 할당하는 경우, 값은 변경되지만 실제 배열의 길이 유지

자바스크립트는 희소 배열을 문법적으로 허용  
희소 배열은 length와 배열 요소의 개수가 일치하지 않음  
희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 큼

배열을 생성할 경우 희소 배열을 생성하지 않도록 주의  
배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선

## 27.4 배열 생성

### 27.4.1 배열 리터럴

객체와 마찬가지로 배열도 다양한 생성 방식 존재  
가장 일반적, 간편한 배열 생성 방식은 배열 리터럴 사용

```js
const arr = [1, 2, 3];
console.log(arr.length); // 3
```

배열 리터럴에 요소를 생략하면 희소 배열 생성

```js
const arr = [1, , 3]; // 희소 배열

// 희소 배열의 length는 배열의 실제 요소 개수보다 언제나 큼
console.log(arr.length); // 3
console.log(arr); // [1, empty, 3]
console.log(arr[1]); // undefined
```

arr\[1]이 undefined인 이유: 객체인 arr에 프로퍼티 키가 '1'인 프로퍼티가 존재하지 않음

### 27.4.2 Array 생성자 함수

Array 생성자 함수를 통해 배열 생성

- 전달된 인수 1개, 숫자인 경우 length 프로퍼티 값이 인수인 배열 생성

  ```js
  const arr = new Array(10);

  console.log(arr); // [empty x 10]
  console.log(arr.length); // 10
  ```

  이때 생성된 배열은 희소 배열(length 프로퍼티의 값은 0이 아니지만 배열의 요소 존재하지 않음)  
  인수의 범위($0$ ~ $2^{23} - 1$)를 벗어나면 RangeError 발생

- 전달된 인수가 없는 경우 빈 배열 생성(배열 리터럴 \[]과 동일)
  ```js
  new Array(); // -> []
  ```
- 전달된 인수 2개 이상, 숫자가 아닌 경우 인수를 요소로 갖는 배열 생성

  ```js
  // 전달된 인수 2개 이상이면 인수를 요소로 갖는 배열 생성
  new Array(1, 2, 3); // -> [1, 2, 3]

  // 전달된 인수 1개, 숫자가 아니면 인수를 요소로 갖는 배열 생성
  new Array({}); // -> [{}]
  ```

  Array 생성자 함수는 new 연산자와 함께 호출하지 않더라도(일반 함수로서 호출해도) 배열을 생성하는 생성자 함수로 동작

  ```js
  Array(1, 2, 3); // -> [1, 2, 3]
  ```

### 27.4.3 Array.of

ES6에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성  
Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성

```js
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성
Array.of(1); // -> [1]
Array.of(1, 2, 3); // -> [1, 2, 3]
Array.of("string"); // -> ['string']
```

### 27.4.4 Array.from

ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환

```js
// 유사 배열 객체를 변환하여 배열을 생성
Array.from({ length: 2, 0: "a", 1: "b" }); // -> ['a', 'b']

// 이터러블을 변환하여 배열을 생성, 문자열을 이터러블
Array.from("Hello"); // -> ['H', 'e', 'l', 'l', 'o']
```

Array.from을 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소 삽입 가능

```js
// Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소로 채움
Array.from({ length: 3 }); // -> [undefined, undefined, undefined]

// Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환
Array.from({ length: 3 }, (_, i) => i); // -> [0, 1, 2]
```

## 27.5 배열 요소의 참조

배열의 요소를 참조할 때 대괄호(\[]) 표기법 사용, 대괄호 안에는 인덱스  
존재하지 않는 요소에 접근하면 undefined 반환(배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 갖고 있는 객체이기 때문)

```js
const arr = [1, 2];

console.log(arr[0]); // 인덱스가 0인 요소를 참조, 1
console.log(arr[2]); // 인덱스가 2인 요소를 참조, undefined
```

## 27.6 배열 요소의 추가와 갱신

객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가 가능  
존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소 추가(length 프로퍼티 값 자동 갱신)

```js
const arr = [0];

arr[1] = 1; // 배열의 요소 추가

console.log(arr); // [0, 1]
console.log(arr.length); // 2
```

## 27.7 배열 요소의 삭제

자바스크립트 배열은 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete 연산자 사용 가능

```js
const arr = [1, 2, 3];

delete arr[1]; // 배열 요소의 삭제
console.log(arr); // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않음. 희소 배열이 됨
console.log(arr.length); // 3
```

delete 연산자는 객체의 프로퍼티를 삭제, 이때 배열은 희소 배열  
희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋음

희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용

```js
const arr = [1, 2, 3];

// Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
// arr[1]부터 1개의 요소를 제거
arr.splice(1, 1);
console.log(arr); // [1, 3]

// length 프로퍼티가 자동으로 갱신
console.log(arr.length); // 2
```

## 27.8 배열 메서드

배열 메서드는 결과물을 반환하는 패턴이 두 가지이므로 주의 필요  
&rarr; 원본 배열을 직접 변경하는 메서드  
&rarr; 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드

```js
const arr = [1];

// push 메서드는 원본 배열(arr)을 직접 변경
arr.push(2);
console.log(arr); // [1, 2]

// concat 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환
const result = arr.concat(3);
console.log(arr); // [1, 2]
console.log(result); // [1, 2 3]
```

원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수 효과가 있으므로 사용 시 주의  
가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋음

### 27.8.1 Array.isArray

Array.isArray는 Array 생성자 함수의 정적 메서드  
해당 메서드는 전달된 인수가 배열이면 true, 아니면 false 반환

```js
Array.isArray([]); // true
Array.isArray({}); // false
```

### 27.8.2 Array.prototype.indexOf

indexOf 메서드는 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스 반환

- 원본 배열에 인수로 전달한 요소와 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 요소의 인덱스 반환
- 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1 반환

```js
const arr = [1, 2, 2, 3];

arr.indexOf(2); // -> 1
arr.indexOf(4); // -> -1
arr.indexOf(2, 2); // -> 2
```

indexOf 메서드는 배열에 특정 요소가 존재하는지 확인할 떄 유용  
ES7에서 도입된 Array.prototype.includes 메서드를 사용하면 가독성 &uarr;

### 27.8.3 Array.prototype.push

push 메서드는 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가  
변경된 length 프로퍼티 값 반환  
해당 메서드는 원본 배열을 직접 변경

```js
const arr = [1, 2];

let result = arr.push(3, 4);
console.log(result); // 4

console.log(arr); // [1, 2, 3, 4]
```

push 메서드는 성능 면에서 좋지 않음  
마지막 요소로 추가할 요소가 하나뿐이면 length 프로퍼티를 사용하는 방법이 보다 빠름

```js
const arr = [1, 2];

arr[arr.length] = 3;
console.log(arr); // [1, 2, 3]
```

push 메서드는 원본 배열을 직접 변경하는 부수 효과가 존재  
따라서 ES6의 스프레드 문법을 사용하는 편이 좋음  
스프레드 문법을 사용하면 함수 호출 없이 표현식으로 마지막에 요소 추가 가능, 부수 효과 없음

```js
const arr = [1, 2];

const newArr = [...arr, 3];
console.log(newArr); // [1, 2, 3]
```

### 27.8.4 Array.prototype.pop

pop 메서드는 원본 배열에서 마지막 요소를 제거  
제거한 요소 반환  
원본 배열이 빈 배열이면 undefined를 반환  
pop 메서드는 원본 배열을 직접 변경

```js
const arr = [1, 2];

let result = arr.pop();
console.log(result); // 2

console.log(arr); // [1]
```

pop 메서드와 push 메서드를 사용하면 스택 구현 가능

**스택**  
데이터를 마지막에 밀어 넣고, 마지막에 밀어 넣은 데이터를 먼저 꺼내는 후입선출(LIFO - Last In First Out) 방식의 자료구조

- push: 스택에 데이터를 밀어 넣는 것
- pop: 스택에서 데이터를 꺼내는 것

### 27.8.5 Array.prototype.unshift

unshift 메서드는 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가  
변경된 length 프로퍼티 값 반환  
해당 메서드는 원본 배열을 직접 변경

```js
const arr = [1, 2];

let result = arr.unshift(3, 4);
console.log(result); // 4

console.log(arr); // [3, 4, 1, 2]
```

unshift 메서드는 원본 배열을 직접 변경하는 부수 효과 존재  
따라서 ES6의 스프레드 문법을 사용하는 편이 좋음  
스프레드 문법을 사용하면 함수 호출 없이 표현식으로 선두에 요소 추가 가능, 부수 효과 없음

```js
const arr = [1, 2];

const newArr = [3, ...arr];
console.log(newArr); // [3, 1, 2]
```

### 27.8.6 Array.prototype.shift

shift 메서드는 원본 배열에서 첫 번째 요소를 제거
제거한 요소 반환
원본 배열이 빈 배열이면 undefined 반환
shift 메서드는 원본 배열을 직접 변경

```js
const arr = [1, 2];

let result = arr.shift();
console.log(result); // 1

console.log(arr); // [2]
```

shift 메서드와 push 메서드를 사용하면 큐 구현 가능

**큐**  
데이터를 마지막에 밀어 넣고, 처음 데이터(가장 먼저 밀어 넣은 데이터)를 먼저 꺼내는 선입 선출(FIFO - First In First Out) 방식의 자료구조  
큐는 언제나 데이터를 밀어 넣은 순서대로 취득

- enqueue: 큐에서 데이터를 밀어 넣는 것
- dequeue: 큐에서 데이터를 꺼내는 것

### 27.8.7 Array.prototype.concat

concat 메서드는 인수로 전달된 값들(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환  
인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가  
원본 배열은 변경되지 않음

```js
const arr1 = [1, 2];
const arr2 = [3, 4];

let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

result = arr1.concat(3);
console.log(result); // [1, 2, 3]

result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

console.log(arr1); // [1, 2]
```

push와 unshift 메서드는 concat 메서드로 대체 가능  
concat 메서드는 ES6의 스프레드 문법으로 대체 가능

```js
let result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

### 27.8.8 Array.prototype.splice

splice 메서드는 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거 하는 경우 사용  
원본 배열 직접 변경

splice 메서드의 매개변수 3개(start, deleteCount, items)

- splice 메서드에 3개의 인수를 빠짐없이 전달하면 시작 인덱스부터 제거할 요소의 개수만큼 원본 배열에서 요소를 제거, 그리고 제거한 위치에 삽입할 요소들을 원본 배열에 삽입

  ```js
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 2개의 요소를 제거, 그 자리에 새 요소 20, 30 삽입
  const result = arr.splice(1, 2, 20, 30);

  // 제거한 요소가 배열로 반환
  console.log(result); // [2, 3]
  // splice 메서드는 원본 배열을 직접 변경
  console.log(arr); // [1, 20, 30, 4]
  ```

- splice 메서드의 두 번째 인수(제거할 요소의 개수)를 0으로 지정하면 아무런 요소를 제거하지 않고 새로운 요소 삽입

  ```js
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 0개의 요소를 제거, 그 자리에 새로운 요소 100 삽입
  const result = arr.splice(1, 0, 100);

  // 원본 배열 변경
  console.log(arr); // [1, 100, 2, 3, 4]
  // 제거한 요소가 배열로 반환
  console.log(result); // []
  ```

- splice 메서드의 세 번째 인수(제거한 위치에 추가할 요소들의 목록)을 전달하지 않으면 원본 배열에서 지정된 요소 제거만 수행

  ```js
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 2개의 요소를 제거
  const result = arr.splice(1, 2);

  // 원본 배열 변경
  console.log(arr); // [1, 4]
  // 제거한 요소가 배열로 반환
  console.log(result); // [2, 3]
  ```

- splice 메서드의 두 번째 인수를 생략하면 첫 번째 인수로 전달된 시작 인덱스부터 모든 요소 제거

  ```js
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 모든 요소를 제거
  const result = arr.splice(1, 2);

  // 원본 배열 변경
  console.log(arr); // [1]
  // 제거한 요소가 배열로 반환
  console.log(result); // [2, 3, 4]
  ```

### 27.8.9 Array.prototype.slice

slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환  
원본 배열은 변경되지 않음

slice 메서드의 매개변수 2개(start, end)

- slice 메서드는 첫 번째 인수(start)로 전달받은 인덱스부터 두 번째 인수(end)로 전달받은 인덱스 이전(end 미포함)까지 요소들을 복사하여 배열로 반환

  ```js
  const arr = [1, 2, 3];

  // arr[0]부터 arr[1] 이전(arr[1] 미포험)까지 복사하여 반환
  arr.slice(0, 1); // -> [1]

  // arr[1]부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환
  arr.slice(1, 2); // -> [2]

  // 원본은 변경되지 않음
  console.log(arr); // [1, 2, 3]
  ```

- slice 메서드의 두 번째 인수를 생략하면 첫 번째 인수로 전달받은 인덱스부터 모든 요소를 복사하여 배열로 반환

  ```js
  const arr = [1, 2, 3];

  // arr[1]부터 이후의 모든 요소를 복사하여 반환
  arr.slice(1); // -> [2, 3]
  ```

- slice 메서드의 첫 번째 인수가 음수인 경우 배열의 끝에서부터 요소를 복사하여 배열로 반환

  ```js
  const arr =. [1, 2, 3];

  // 배열의 끝에서부터 요소를 한 개 복사하여 반환
  arr.slice(-1); // -> [3]

  // 배열의 끝에서부터 요소를 두 개 복사하여 반환
  arr.slice(-2); // -> [2, 3]
  ```

- slice 메서드의 인수를 모두 생략하면 원본 배열의 복사본(얕은 복사)을 생성하여 반환

  ```js
  const arr = [1, 2, 3];

  // 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환
  const copy = arr.slice();
  console.log(arr); // [1, 2, 3]
  console.log(copy === arr); // false
  ```

### 27.8.10 Array.prototype.join

join 메서드는 원본 배열의 모든 요소를 문자열로 반환 후, 인수로 전달받은 문자열(구분자)로 연결한 문자열을 반환  
구분자 생략 가능, 기본 구분자는 콤마(,)

```js
const arr = [1, 2, 3, 4];

arr.join(); // -> `1,2,3,4`;
arr.join(""); // -> `1234`
arr.join(":"); // -> `1:2:3:4`
```

### 27.8.11 Array.prototype.reverse

reverse 메서드는 원본 배열의 순서를 반대로 뒤집음  
이때 원본 배열이 변경  
반환값은 변경된 배열

```js
const arr = [1, 2, 3];
const result = arr.reverse();

// reverse 메서드는 원본 배열을 직접 변경
console.log(arr); // [3, 2, 1]
// 반환값은 변경된 배열
console.log(result); // [3, 2, 1]
```

### 27.8.12 Array.prototype.fill

ES6에서 도입된 fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채움  
원본 배열은 변경

- 첫 번째 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채움

  ```js
  const arr = [1, 2, 3];

  arr.fill(0);

  console.log(arr); // [0, 0, 0]
  ```

- 두 번째 인수로 요소 채우기를 시작할 인덱스를 전달

  ```js
  const arr = [1, 2, 3];

  arr.fill(0, 1);

  console.log(arr); // [1, 0, 0]
  ```

- 세 번째 인수로 요소 채우기를 멈출 인덱스를 전달

```js
const arr = [1, 2, 3, 4, 5];

arr.fill(0, 1, 3);

console.log(arr); // [1, 0, 0, 4, 5]
```

### 27.8.13 Array.prototype.includes

ES7에서 도입  
includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환

- 첫 번째 인수로 검색할 대상을 지정

  ```js
  const arr = [1, 2, 3];

  arr.includes(2); // -> true
  arr.includes(100); // -> false
  ```

- 두 번째 인수로 검색을 시작할 인덱스 전달(생략 시, 기본값 0)

  ```js
  const arr = [1, 2, 3];

  arr.includes(1, 1); // -> false
  arr.includes(3, -1); // -> true
  ```

### 27.8.14 Array.prototype.flat

ES10에서 도입  
flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화  
중첩 배열을 평탄화할 깊이를 인수로 전달(생략 시, 기본값 1)  
인수로 Infinity를 전달하면 중첩 배열을 모두 평탄화

```js
[1, [2, 3, 4, 5]].flat(); // -> [1, 2, 3, 4, 5]
[1, [2, [3, [4]]]].flat(); // -> [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(2); // -> [1, 2, 3, [4]]
[1, [2, [3, [4]]]].flat(Infinity); // -> [1, 2, 3, 4]
```

## 27.9 배열 고차 함수

고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수  
자바스크립트의 함수는 일급 객체이므로 함수를 값처럼 인수로 전달, 반환 가능  
고차 함수는 외부 상태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 둠

**함수형 프로그래밍**

- 순수 함수, 보조 함수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성 해결
- 변수의 사용을 억제하여 상태 변경을 지양
- 순수 함수를 통해 부수 효과를 최대한 억제
- 오류를 피하고 프로그램의 안정성 상승 기대

### 27.9.1 Array.prototype.sort

sort 메서드는 배열의 요소를 정렬  
원본 배열을 직접 변경하며 정렬된 배열을 반환  
기본적으로 오름차순으로 요소 정렬  
내림차순으로 요소를 정렬하려면 오름차순 정렬 후, reverse 메서드를 사용

```js
const fruits = ["Banana", "Orange", "Apple"];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경
console.log(fruits); // ['Apple', 'Banana', 'Orange']

// 내림차순(descending) 정렬
fruits.reverse();

// reverse 메서드도 원본 배열을 직접 변경
console.log(fruits); // ['Orange', 'Banana', 'Apple']
```

문자열 요소로 이루어진 배열의 정렬은 이상 없음

숫자 요소로 이루어진 배열을 정렬할 때 주의 필요

```js
const points = [40, 100, 1, 5, 2, 25, 10];

points.sort();

console.log(points); // [1, 10, 100, 2, 25, 40, 5]
```

sort 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따름  
따라서 숫자 요소를 정렬할 때는 sort 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달 필요

**비교 함수**

- 양수나 음수 또는 0을 반환
- 반환값: 음수 &rarr; 비교 함수의 첫 번째 인수를 우선하여 정렬
- 반환값: 0 &rarr; 정렬하지 않음
- 반환값: 양수 &rarr; 비교 함수의 두 번째 인수를 우선하여 정렬

```js
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬, 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열의 내림차순 정렬, 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]
```

### 27.9.2 Array.prototype.forEach

조건문, 반복문은 로직의 흐름을 이해하기 어렵게 함  
for 문은 함수형 프로그래밍이 추구하는 바와 맞지 않음

forEach 메서드는 for 문을 대체할 수 있는 고차 함수, 자신의 내부에서 반복문 실행

```js
const numbers = [1, 2, 3];
const pows = [];

// forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
numbers.forEach((item) => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
```

forEach 메서드의 반환값은 언제나 undefined  
forEach 메서드는 for 문과 달리 break, continue 문 사용 불가  
for문에 비해 성능 &darr;, 가독성 &uarr;

### 27.9.3 Array.prototype.map

map 메서드는 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출  
콜백 함수의 반환값들로 구성된 새로운 배열 반환  
원본 배열은 변경되지 않음

```js
const numbers = [1, 4, 9];

const roots = numbers.map((item) => Math.sqrt(item));

// map 메서드는 새로운 배열 반환
console.log(roots); // [ 1, 2, 3 ]
// map 메서드는 원본 배열을 변경하지 않음
console.log(numbers); // [ 1, 4, 9 ]
```

### 27.9.4 Array.prototype.filter

filter 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출  
콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열 반환  
원본 배열은 변경되지 않음

```js
const numbers = [1, 2, 3, 4, 5];

const odds = numbers.filter((item) => item % 2);
console.log(odds); // [1, 3, 5]
```

filter 메서드는 자신을 호출한 배열에서 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 만들고 싶을 때 사용  
자신을 호출한 배열에서 특정 요소를 제거하기 위해 사용 가능

### 27.9.5 Array.prototype.reduce

reduce 메서드는 자신을 호출한 배열을 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출  
콜백 함수의 반환값을 다음 순회 시, 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환  
원본 배열은 변경되지 않음

```js
// 1 부터 4까지 누적을 구함
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0
);

console.log(sum); // 10
```

reduce 메서드는 자신을 호출한 배열의 모든 요소를 순회하며 하나의 결과값을 구해야 하는 경우에 사용

reduce 메서드의 다양한 활용법

- 평균 구하기

  ```js
  const values = [1, 2, 3, 4, 5, 6];

  const average = values.reduce((acc, cur, i, { length }) => {
    // 마지막 순회가 아니면 누적값을 반환, 마지막 순회면 누적값의 평균 반환
    return i === length - 1 ? (acc + cur) / length : acc + cur;
  }, 0);

  console.log(average); // 3.5
  ```

- 최대값 구하기

  ```js
  const values = [1, 2, 3, 4, 5];

  const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0);

  console.log(max); // 5
  ```

  Math.max 메서드를 사용하는 방법이 더 직관적

- 요소의 중복 횟수 구하기

  ```js
  const fruits = ["banana", "apple", "orange", "orange", "apple"];

  const count = fruits.reduce((acc, cur) => {
    acc[cur] = (acc[cur] || 0) + 1;
    return acc;
  }, {});
  /*
  {banana: 1}
  => {banana: 1, apple: 1}
  => {banana: 1, apple: 1, orange: 1}
  => {banana: 1, apple: 1, orange: 2}
  => {banana: 1, apple: 2, orange: 2}
  */

  console.log(count); // {banana: 1, apple: 2, orange: 2}
  ```

- 중첩 배열 평탄화

  ```js
  const values = [1, [2, 3], 4, [5, 6]];

  const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
  /*
  [1]
  => [1, 2, 3]
  => [1, 2, 3, 4]
  => [1, 2, 3, 4, 5, 6]
  */
  console.log(flatten); // [1, 2, 3, 4, 5, 6]
  ```

- 중복 요소 제거

  ```js
  const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

  const result = values.reduce(
    (unique, val, i, _values) =>
      _values.indexOf(val) === i ? [...unique, val] : unique,
    []
  );

  console.log(result); // [1, 2, 3, 4, 5]
  ```

  filter 메서드를 사용하는 방법이 더 직관적  
  중복되지 않는 유일한 값의 집합 Set을 사용하는 것을 추천

### 27.9.6 Array.prototype.some

some 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출  
some 메서드는 콜백 함수의 반환값이 단 한 번이라도 참이면 true, 모두 거짓이면 false 반환  
호출한 배열이 빈 배열인 경우 언제나 false 반환

```js
[5, 10, 15].some((item) => item > 10); // -> true
[5, 10, 15].some((item) => item < 0); // -> false
[].some((item) => item > 3); // -> false
```

### 27.9.7 Array.prototype.every

every 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출  
이때 every 메서드는 콜백 함수의 반환 값이 모두 참이면 true, 단 한 번이라도 거짓이면 false 반환  
호출한 배열이 빈 배열인 경우 언제나 true 반환

```js
[5, 10, 15].every((item) => item > 3); // -> true
[5, 10, 15].every((item) => item > 10); // -> false
[].every((item) => item > 3); // -> true
```

### 27.9.8 Array.prototype.find

ES6에서 도입  
find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환  
콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined 반환

```js
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
  { id: 3, name: "Choi" },
  { id: 4, name: "Park" },
];

users.find((user) => user.id === 2); // -> { id: 2, name: 'Kim' }
```

find의 결과값은 배열이 아닌 해당 요소값

### 27.9.9 Array.prototype.findIndex

ES6에서 도입  
findIndex 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스 반환  
콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1 반환

```js
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
  { id: 3, name: "Choi" },
  { id: 4, name: "Park" },
];

users.findIndex((user) => user.id === 2); // -> 1
users.findIndex((user) => user.name === "Park"); // -> 3
```

### 27.9.10 Array.prototype.flatMap

ES10에서 도입  
flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화  
map 메서드와 flat 메서드를 순차적으로 실행하는 효과 (1단계 평탄화 가능)

```js
const arr = ["hello", "world"];

// map과 flat을 순차적으로 실행
arr.map((x) => x.split("")).flat();
// -> ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

// flatMap은 map을 통해 생성된 새로운 배열을 평탄화
arr.flatMap((x) => x.split(""));
// -> ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```
