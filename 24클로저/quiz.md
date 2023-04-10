### [1]

```jsx
const x = 1;

function outer(){
	const x = 10;
	const inner = function(){console.log(x);};
	return inner;
}

const innerFunc = outer();
innerFunc(); //?
```
<details>
<summary>답</summary>
<div markdown="1">
아니다!
  <details>
  <summary>해설</summary>
  <div markdown="1">
    - innerFunc을 호출하면 중첩함수 inner를 반환하고 생명주기를 마감 </br>
    - innerFunc실행이 종료되면 innerFunc의 실행 컨텍스트는 실행컨텍스트 스택에서 제거</br>
    - innerFunc의 지역변수와 x와 변수의 값 10을 저장하고 있던 innerFunc의 실행컨텍스트가 제거되었으므로 innerFunc의 지역변수 x또한 생명주기를 마감</br>
    - 따라서 innerFunc의 지역변수 x는 더는 유효하지 않게 되어 x변수에 접근할 수 있는 방법은 없다</br>
    ❗BUT</br>
    - 위 코드의 실행 결과 innerFunc()는 innerFunc의 지역변수 x의 값인 10이다. 이미 생명주기가 종료되어 실행 컨텍스트 스택에서 제거된 innerFunc의 지역변수가 다시 동작하고 있음</br>
    - 이처럼 외부 함수보다 중첩함수가 더 오래 유지되는 경우 중첩함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조</br>
    - 이러한 중첩 함수를 클로저라고 부른다.</br>
  </div>
  </details>
</div>
</details>


### [2] 아래 코드를 보고 답하기

```jsx
function foo(){
    const x = 1;
    const y = 2;

		//문제! 다음 함수는 클로저라고 할 수 있나?
    function bar(){
	    const z =3;
	    debugger;
	    console.log(z);
    }
    return bar;
}
const bar = foo();
bar();
```

<details>
<summary>답</summary>
<div markdown="1">
아니다!
  <details>
  <summary>해설</summary>
  <div markdown="1">
    - 중첩함수 bar는 외부 함수 foo보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않음</br>
    - 상위 스코프의 어떤 식별자도 참조하지 않은 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않음</br>
    - 참조하지 않는 식별자를 기억하는 것은 메모리 낭비이기 때문</br>
  </div>
  </details>
</div>
</details>



### [3] 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 _____하고

### 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용

<details>
<summary>답</summary>
<div markdown="1">
  은닉
</div>
</details>
