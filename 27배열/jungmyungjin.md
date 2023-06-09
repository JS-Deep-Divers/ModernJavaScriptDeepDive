# 2023.03.30 자바스크립트 딥다이브 (27.배열)

## 기억하고 싶은 부분

### 자바스크립트에서의 배열

- 자바스크립트에서의 배열은 **해시 테이블로 구현된 객체**이므로 일반적인 배열보다 성능적인 면에서 느릴수밖에 없는 구조적인 단점이 있다.
- 하지만 **특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능**을 기대할 수 있다.
- 최적화가 잘 되어있는 모던 자바스크립트 엔진은 요소의 타입이 일치하는 배열을 생성할 때 일반적인 의미의 배열처럼 연속된 메모리 공간을 확보한다.
  따라서 **배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다**



### 희소배열(sparse array)

배열의 요소가 연속적으로 이어져 있지 않은 배열

- 현재 `length 프로퍼티`값 보다 큰 숫자를 할당 한 경우, `length 프로퍼티`값은 변경되지만 **실제 배열의 길이가 늘어나지는 않는다.**
- 희소 배열의 `length`는 희소 배열의 요소 개수보다 언제나 크다.
- 배열을 생성할 경우 희소배열을 생성하지 않도록 주의해야한다.



### 배열의 요소 다루기

- 존재하지 않는 요소에 접근하면 `undefined`가 반환된다.

- 현재 배열의 크기보다 큰 인덱스에 요소를 추가하면 희소 배열이 된다.

  ```javascript
  arr[100] = 100;
  console.log(arr); // [empty, empty ... 100];
  // 이때 요소에 접근하여 명시적으로 값을 할당하지 않는 요소들(0~99번째 인덱스)은 생성되지 않는다.
  ```

- 인덱스 요소는 **0이상의 정수**를 사용해야 한다.

- **그 이외의 값들을 사용하면 프로퍼티가 생성**되며, 이때 추가된 프로퍼티는 **length 프로퍼티 값에 영향을 주지 않는다.**

- 배열은 객체이기 때문에 `delete`연산자를 사용할 수 있다.

  - `delete`연산을 사용하면 요소의 값은 삭제되지만, `length 프로퍼티`값은 변하지 않는다.
  - 때문에 특정 요소를 완전히 삭제하려면 `splice`메서드를 사용해야 한다.



### 배열의 메서드

**concat**

- 인수로 전달된 값(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]
```



**fill** (ES6)

- 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다

```javascript
const arr = [1, 2, 3];
arr. fill(0); // [0, 0, 0]
```



**flat** (ES10)

- 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화 한다

```javascript
// 기본값은 1이다
[1, [2, [3, [4]]]].flat(); // -> [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(1); // -> [1, 2, [3, [4]]]

[1, [2, [3, [4]]]].flat(2); // -> [1, 2, 3, [4]]

[1, [2, [3, [4]]]].flat(Infinity); // -> [1, 2, 3, 4]
```



**flatmap** (EX10)

- map과 flat을 순차적으로 실행한다
- 단, `flat` 깊이를 1단계만 평탄화한다.
- 평탄화 깊이를 지정해야 한다면,  `map()`과 `flat()`메서드를 각각 호출해야 한다.

```javascript
const arr = ['hello', 'world'];
arr.map(x=> x.split('')).flat(); // ['h','e','l','l','o','w','o','r','l','d']
arr.flatMap(x=> x.split('')); // ['h','e','l','l','o','w','o','r','l','d']
```





## 느낀점

- 이제서야 배열을..? 인 생각이 들긴하지만 깊이있게 알 수 있어서 좋았다

- 저급 언어인 C언어를 하고 고급언어인 자바스크립트를 배우니 이해되는 부분이 있었다. 만약 고급 언어부터 배웠더라면 모르고 넘겼을 부분이 있었을것 같다.

- 배열 메서드 중 처음 보는 메서드를 몇개 알았다. 특히 flat은 매우 유용할 것 같다.

  