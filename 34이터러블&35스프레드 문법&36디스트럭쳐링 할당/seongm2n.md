### 34

[34.1] 이터레이션 프로토콜

- for…of문, 스프레드 문법, 배열 디스트럭처 할당의 대상으로 사용할 수 있게 일원화함

(2) 이터레이터

- 이터러블의 Symbol.iterator메서드가 반환한 이터레이터는 next메서드 갖음

[34.2] 빌드인 이터러블

| 빌트인 이터러블 | Symbol.iterator 메서드 |
| --- | --- |
| Array | Array.prototype[Symbol.iterator] |
| String | String.prototype[Symbol.iterator] |
| Map | Map.prototype[Symbol.iterator] |
| Set | Set.prototype[Symbol.iterator] |
| TypedArray | TyedArray.prototype[Symbol.iterator] |
| arguments | arguments[Symbol.iterator] |
| DOM 컬렉션 | NodeList.prototype[Symbol.iterator]
HTMLCollection.prototype[Symbol.iterator] |

[34.6] 사용자 정의 이터러블

(4) 무한 이터러블과 지연 평가

- 이터러블은 데이터 공급자의 역할을 함
- 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터 공금

### 35

- 스프레드 문법을 사용할 수 있는 대상 : Array, String, Map, Set, DOM컬렉션, arguments와 같이 for…of문으로 순회할 수 있는 이터러블에 한정
- 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없음

```jsx
console.log(...{a:1,b:2})}; //typeError
```

- 스프레드 문법의 결과는 변수에 할당 할 수 없음
- 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용
    - 함수 호출문의 인수 목록 → Rest 파라미터와 스프레드 문법은 서로 반대 개념
    - 배열 리터럴의 요소 목록
        - concat : 2개의 배열을 1개의 배열로 결합하고 싶은 경우

            ```jsx
            var arr = [1,2].concat([3,4]);
            console.log(arr); // [1,2,3,4]

            //스프레드 문법 사용
            const arr = [...[1,2], ...[3,4]];
            console.log(arr); //[1,2,3,4]
            ```

        - splice: 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거

            ```jsx
            var arr1 = [1,4];
            var arr2 = [2,3];
            //arr2를 해체하여 전달해야 함
            //그렇지 않으면 arr1과 arr2배열 자체가 추가
            arr1.splice(1,0,arr2); //[1,[2,3],4]

            //Function.prototype.apply메서드를 사용하여 splice메서드 호출
            var arr1 = [1,4];
            var arr2 = [2,3];

            /*
            apply 메서드의 2번째 인수(배열)는 apply메서드가 호출한 splice메서드의 인수 목록
            apply 메서드의 2번째 인수 [1,0].concat(arr2)는 [1,0,2,3]으로 평가
            따라서 splice메서드에 apply메서드의 2번째 인수 [1,0,2,3]이 해체되어 전달
            즉, arr1[1]부터 0개의 요소를 제거하고 그 자리(arr[1])에 새로운 요소(2,3)를 삽입
            */

            Array.prototype.splice.apply(arr1, [1,0].concat(arr2));
            console.log(arr1); // [1,2,3,4]
            ```

        - 배열 복사 → slice
        - 이터러블을 배열로 변환 → 이터러블을 배열로 변환하려면 Function.prototype.apply 또는 Function.prototype.call메서드 사용하여 slice메서드 호출
    - 객체 리터럴의 프로퍼티 목록
        - 스프레드 프로퍼티는 Object.assign메서드를 대체할 수 있는 간편한 문법

### 36

디스트럭처링(구조분해) 할당

- 할당의 대상(할당문의 우변)은 이터러블, 할당 기준은 배열의 인덱스
- 우변에 이터러블을 할당하지 않으면 에러 발생

```jsx
const arr = [1,2,3];

//변수 one, two, three 선언, 배열 arr을 디스트럭처링 하여 할당
//이때의 할당 기준은 배열의 인덱스
const [one,two,three] = arr;
console.log(one,two,three); //1,2,3
```

```jsx
const user = {firstName: 'seongmin', lastName:'shin'};
//변수 lastName,firstName을 선언하고 user객체를 디스트럭처링하여 할당
//프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어짐, 순서는 의미가 없음
const {lastName, firstName} = user;
console.log(firstName, lastName); //seongmin shin
```
