### 37

[37.1] set

- set : 중복되지 않는 유일한 값들의 집합

| 구분 | 배열 | Set객체 |
| --- | --- | --- |
| 동일한 값을 중복하여 포함할 수 있다. | ⭕️ | ❌ |
| 요소 순서에 의미가 있다 | ⭕️ | ❌ |
| 인덱스로 요소에 접근할 수 있다. | ⭕️ | ❌ |
- set 객체의 특성 = 수학적 집합의 특성
    - 교집합(intersection), 합집합(union), 차집합(difference), 여집합(isSuperset)
- 중복된 값은 set객체에 요소로 저장되지 않음

```jsx
//set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2,1,2,3,4,3,4])); //[2,1,3,4]
```

(5) 요소 삭제

- delete메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환
- Set.prototype.add메서드와 달리 연속적으로 호출 할 수 없음

```jsx
const set = new Set([1,2,3]);
set.delete(1).delete(2); // typeerror
```

(7) 요소 순회

- 첫 번째 인수 : 현재 순회 중인 요소값, 두 번째 인수: 현재 순회 중인 요소값, 세 번째 인수: 현재 순회 중인 Set 객체 자체
- 첫 번째 인수와 두 번째 인수는 같은 값

    → Array.prototype.forEach 메서드와 인터페이스를 통일하기 위함이며 의미 없음

    → Array.prototype.forEach 메서드의 콜백 함수는 두 번째 인수로 현재 순회 중인 요소의 인덱스를 전달받음

    → Set객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않음

    ```jsx
    const set = new Set([1,2,3]);
    set.forEach((v,v2,set)=>console.log(v,v2,set));
    /*
    1 1 Set(3){1,2,3}
    2 2 Set(3){1,2,3}
    3 3 Set(3){1,2,3}
    */
    ```

- Set객체는 이터러블 → for…of문으로 순회, 스프레드 문법과 배열 디스트럭처링의 대상이 됨

[37.2] Map

- Map객체는 키와 값의 쌍으로 이루어진 컬렉션

| 구분 | 객체 | Map객체 |
| --- | --- | --- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값 | 객체를 포함한 모든 값 |
| 이터러블 | ❌ | ⭕️ |
| 요소 개수 확인 | Object.keys(obj).length | map.size |
- Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써짐
- set메서드와 달리 연속적으로 호출할 수 없음
- Map 객체는 이터러블 → for…of문으로 순회, 스프레드 문법과 배열 디스트럭처링의 대상이 됨

### 42

[42.1] 동기 처리와 비동기 처리

- 자바크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖음
- 동기 처리(sync): 자바스크립트 엔진은 한 번에 하나의 태스크만 실행할 수 있는 싱글 스레드 방식으로 동작
    - 장점: 태스크를 순서대로 하나씩 처리하므로 실행 순서가 보장됨
    - 단점: 앞선 태스크가 종료할 때까지 이후 태스크들이 블로킹됨
- 비동기 처리(async):  현재 실행 중인 태스크가 종요되지 않은 상태라 해도 다음 태스크를 실행하는 방식
    - 장점 : 현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하므로 블로킹이 발생하지 않음
    - 단점 : 태스크의 실행 순서가 보장되지 않음
    - 콜백 패턴 사용
    - 타이머 함수인 setTimeout과 setInterval, HTTP요청, 이벤트 핸들러

[42.2] 이벤트 루프와 태스크 큐

- 이벤트 루프 : 동시성을 지원하는 것
- 태스크가 요청되면 콜 스택을 통해 요청된 작업을 순차적으로 실행
- 타이머 설정과 콜백 함수의 등록은 브라우저 또는 Node.js가 담당 → 브라우저 환경은 태스크 큐와 이벤트 루프 제공

**🎀자바스크립트 엔진은 싱글 스레드로 동작하지만 브라우저는 멀티 스레드로 동작**
