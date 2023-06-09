### 24

사전지식 : 실행컨텍스트

MDN정의 : *“A closure is the combination of a function and the lexical environment within which that function was declared” - 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.*

[24.1] 렉시컬 스코프(정적스코프)

- 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정

[24.3] 클로저와 렉시컬 환경

```jsx
const x = 1;
//(1)
function outer(){
	const x = 10;
	const inner = function(){console.log(x);}; //(2)중첩함수 inner
	return inner;
}

const innerFunc = outer(); //(3)
innerFunc(); //(4)
```

- (3)을 호출하면 중첩함수(2)를 반환하고 생명주기를 마감
- (3)실행이 종료되면 (3)의 실행 컨텍스트는 실행컨텍스트 스택에서 제거
- (3)의 지역변수와 x와 변수의 값 10을 저장하고 있던 (3)의 실행컨텍스트가 제거되었으므로 (3)의 지역변수 x또한 생명주기를 마감
- 따라서 (3)의 지역변수 x는 더는 유효하지 않게 되어 x변수에 접근할 수 있는 방법은 없다

❗BUT

- 위 코드의 실행 결과(4)는 (3)의 지역변수 x의 값인 10이다. 이미 생명주기가 종료되어 실행 컨텍스트 스택에서 제거된 (3)의 지역변수가 다시 동작하고 있음
- 이처럼 외부 함수보다 중첩함수가 더 오래 유지되는 경우 중첩함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조
- 이러한 중첩 함수를 클로저라고 부른다.

---

- 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저!

---

- 하지만 일반적으로 모든 함수를 클로저라고 하지는 않음

```jsx
function foo(){
    const x = 1;
    const y = 2;

		//closure라고 할 수 없음!!!!!
    function bar(){
	    const z =3;
	    debugger;
	    console.log(z);
    }
    return bar;
}
const bar = foo();
bar(); // 3
```

- 중첩함수 bar는 외부 함수 foo보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않음
- 상위 스코프의 어떤 식별자도 참조하지 않은 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않음
- 참조하지 않는 식별자를 기억하는 것은 메모리 낭비이기 때문

---

```jsx
function foo(){
    const x = 1;

    function bar(){
	    debugger;
	    console.log(x);
    }
    bar();
}
foo();

function foo(){
    const x = 1;
    const y = 2;

    function bar(){
	    debugger;
	    console.log(x); //x만 참조될 경우 최적화를 통해 클로저가 참조하고 있는 식별자만을 기억
	  }
	   return bar;
}
const bar = foo();
bar();
```

- bar는 상위스코프를 참조하고 있으므로 클로저이다.
- 외부 함수의 외부로 반환되어 외부 함수보다 더 오래 살아 남음

---

**❗클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적임**

❕클로저에 의해 참조되는 상위 스코프의 변수 → 자유 변수

[24.4]클로저의 활용

- 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용
- 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에만 상태 변경을 허용
- 변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있음
- 외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저 적극 사용

[24.5]캡슐화와 정보 은닉

- 캡슐화
    - 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말함
    - 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라고 함
        - 정보은닉이란 외부에 공개할 필요 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과

[24.6] 자주 발생하는 실수

- 반복문의 코드 블록 내부에서 함수를 정의가 없는 반복문이 생성하는 새로운 렉시컬 환경은 반복 직후, 아무것도 참조하지 않기 때문에 가비지 컬렉션으 대상
- 함수형 프로그래밍 기법인 고차 함수 사용 → 변수와 반복문의 사용을 억제할 수 있기에 오류를 줄이고 가독성을 좋게 만듦
