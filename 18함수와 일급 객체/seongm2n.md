### 18장

[18.1] 일급 객체

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능
2. 변수나 자료구조(객체,배열 등)에 저장
3. 함수의 매개변수에 전달
4. 함수의 반환값으로 사용

```jsx
//1. 무명의 리터럴로 생성
//2. 변수에 저장
const increase = function(num){
	return ++num;
};
const decrease = function(num){
	return --num;
}
// 2.자료구조(객체,배열 등)에 저장
const auxs = {increase, decrease};

//3. 함수의 매개변수에 전달
//4. 함수의 반환값으로 사용
function makeCounter(aux){
	let num =0;
	return function(){
		num = aux(num);
		return num;
	};
}
//3. 함수의 매개변수에 전달
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2
//4. 함수의 반환값으로 사용
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2

```

❗일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다.

[18.2] 함수 객체의 프로퍼티

- 함수는 객체 → 프로퍼티 가질 수 있음
- 콘솔로 함수 객체 내부 보기 ( `console.dir`)

(1) arguments(인수) 프로퍼티

- 함수 호출시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체
- 함수 내부에서 지역 변수처럼 사용
- 함수 외부에서는 참조 불가
- ES6부터 arguments 객체는 유사 배열 객체이면서 동시에 이터러블

<reduce()메서드>

- 배열의 각 요소에 대해 주어진 리듀서 함수 실행 후 하나의 결과값 반환
- 리듀서 함수는 네 개의 인자를 가짐
    1. 누산기(acc)
    2. 현재 값(cur)
    3. 현재 인덱스(idx)
    4. 원본 배열(src)

    구문

    <aside>
    💡 `arr.reduce(callback[, initialValue])`

    </aside>

    - callback : 배열의 각 요소에 대해 실행할 함수, 네가지 인수 받음
        1. 누산기(accumulator) : 콜백의 반환값을 누적, 콜백의 이전 반환값 또는 콜백의 첫 번째 호출 초기값
        2. 현재 값(currentValue) : 처리할 현재 요소
        3. 현재 인덱스(currentIndex) : 처리할 현재 요소의 인덱스
        4. 원본 배열(array) : reduce()를 호출한 배열

        ```jsx
        var maxCallback = ( acc, cur ) => Math.max( acc.x, cur.x );
        var maxCallback2 = ( max, cur ) => Math.max( max, cur );

        // initialValue 없이 reduce()
        [ { x: 22 }, { x: 42 } ].reduce( maxCallback ); // 42
        [ { x: 22 }            ].reduce( maxCallback ); // { x: 22 }
        [                      ].reduce( maxCallback ); // TypeError

        // map/reduce로 개선 - 비었거나 더 큰 배열에서도 동작함
        [ { x: 22 }, { x: 42 } ].map( el => el.x )
                                .reduce( maxCallback2, -Infinity );
        ```


(5)__proto__접근자 프로퍼티

- 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티
- 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근
- __proto__접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근
