### 27. 배열

- 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.

**일반적인 배열과 자바스크립트의 배열의 장단점**

- 일반적인 배열은 인덱스로 요소에 빠르게 접근할 수 있다. 하지만 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.

- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수 밖에 없는 구조적인 단점이 있다. 하지만 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

## length 프로퍼티와 희소 배열

```js
const arr = [1];

// 현재 length 프로퍼티 값인 1보다 큰 숫자 3을 length 프로퍼티에 할당
arr.length = 3;

// length 프로피티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty*2]
```

## 배열 생성

# Array 생성자 함수

- Object 생성자 함수를 통해 객체를 생성할 수 있듯이 Array 생성자 함수를 통해 배열을 생성할 수 있다. Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작하므로 주의가 필요하다.

```js
// 전달된 인수가 1개이고 숫자인 경우, length 프로퍼티 값이 인수인 배열을 생성한다.

const arr = new Array(10);

console.log(arr); // [empty * 10]
console.log(arr.length); // 10
```

# Array.of

```js
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
Array.of(1); // [1]

Array.of(1, 2, 3); // [1,2,3]

Array.of("string"); // ['string']
```

# Array.from

```js
//유사 배열 객체를 변환하여 배열을 생성한다.
Array.from({ length: 2, 0: "a", 1: "b" }); // ['a', 'b']

// 이터러블을 변환하여 배열을 생성한다. 문자열은 이터러블이다.
Array.from("Hello"); // ['H', 'e', 'l', 'l', 'o']
```

```js
// Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소로 갖는다
Array.from({ length: 3 }); // [undefined,undefined,undefined]

// Arrau.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
Array.from({ length: 3 }, (_, i) => i); // [0,1,2]
```

## 배열 메서드

- 배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.

**Array.isArray**
=> 전달된 인수가 배열이면 true, 배열이 아니면 false를 반환한다.

**Array.prototype.indexOf**
=> indexOf 메서드는 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.
=> 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1을 반환한다.

**Array.prototype.push**
=> push 메서드는 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.
=> 원본 배열을 직접 변경한다.

**Array.prototype.pop**
=> pop메서드는 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
=> 원본 배열을 직접 변경한다.

**Array.prototype.unshift**
=> unshift 메서드는 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.
=> 원본 배열을 직접 변경한다.

**Array.prototype.shift**
=> shift 메서드는 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
=> 원본 배열이 빈 배열이면 undefined를 반환한다.
=> 원본 배열을 직접 변경한다.

**Array.prototype.concat**
=> concat 메서드는 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.
=> 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.
=> 원본 배열은 변경되지 않는다.

**Array.prototype**
=> 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다.
=> 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.

**Array.prototype.slice**
=> slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.
=> 원본 배열은 변경되지 않는다.

**Array.prototype.join**
=> join 메서드는 원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다.
=> 구분자는 생략 가능하며 기본 구분자는 콤마다.

**Array.prototype.reverse**
=> reverse 메서드는 원본 배열의 순서를 반대로 뒤집는다.
=> 이때, 원본 배열이 변경된다.

**Array.prototype.fill**
=> fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.
=> 이떄, 원본 배열이 변경된다.

**Array.prototype.includes**
=> includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환한다.
=> 첫 번째 인수로 검색할 대상을 지정한다.

**Array.prototype.flat**
=> flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```js
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]
```

## 배열 고차 함수

**Array.prototype.sort**
=> sort 메서드는 배열의 요소를 정렬한다.
=> 원본 배열을 직접 변경하며 정렬된 배열을 반환한다.
=> 기본적으로 오름차순으로 요소를 정렬한다.

**Array.prototype.forEach**
=> forEach 메서드는 for문을 대체할 수 있는 고차 함수다.
=> 자신의 내부에서 반복문을 실행한다.

**Array.prototype.map**
=> map 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
=> 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
=> 이떄, 원본 배열은 변경되지 않는다.

**Array.prototype.filter**
=> filter 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
=> 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.
=> 이때 원본 배열은 변경되지 않는다.

**Array.prototype.reduce**
=> reduce 메서드는 자신을 호출한 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다.
=> 콜백 함수의 반환값을 다음 순회시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환한다,
=> 이떄, 원본 배열은 변경되지 않는다.

```js
// 1부터 4까지 누적을 구한다.
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0
);

console.log(sum); // 10
```

**Array.prototype.some**
=> some 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다.
=> some 메서드는 콜백 함수의 반환값이 단 한번이라도 참이면 true, 모두 거짓이면 false를 반환한다.
=> some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환한다.

**Array.prototype.every**
=> every 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다.
=> every 메서드는 콜백 함수의 반환값이 모두 참이면 true, 단 한번이라도 거짓이면 false를 반환한다.
=> every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환한다.

**Array.prototype.find**
=> find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다.
=> 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined를 반환한다.

**Array.prototype.findIndex**
=> findIndex 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다.
=> 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1을 반환한다.

**Array.prototype.flatMap**
=> flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화한다.
=> map 메서드와 flat 메서드를 순차적으로 실행하는 효과가 있다.
