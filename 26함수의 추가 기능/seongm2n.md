[26.1] 함수의 구분

- ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있음
- ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성
- 문제 해결을 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| --- | --- | --- | --- | --- |
| 일반 함수(Normal) | ⭕️ | ⭕️ | ❌ | ⭕️ |
| 메서드(Method) | ❌ | ❌ | ⭕️ | ⭕️ |
| 화살표 함수(Arrow) | ❌ | ❌ | ❌ | ❌ |

[26.2] 메서드

- ES6 이전 사양에는 메서드에 대한 명확한 정의가 없음
- ES6에서의 메서드는 메서드 축약 표현으로 정의된 함수만을 의미
- ES6메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯을 갖음
- 메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋음

[26.3] 화살표 함수

- function 키워드 대신 화살표(⇒,fat arrow)를 사용하여 기존 함수 정의 방식보다 간략하게 함수 정의
- 표현만 간략한 것이 아니라 내부 동작도 기존의 함수보다 간략

(1) 화살표 함수 정의

- 함수 정의

```jsx
const multiply = (x, y) => x * y;
multiply(2,3);
```

- 매개변수 선언

```jsx
//매개변수가 여러 개인 경우 소괄호()안에 매개변수를 선언
const arrow = (x,y) => {...};

//매개변수가 한 개인 경우 소괄호()를 생략 가능
const arrow = x =>{...};

//매개변수가 없는 경우 소괄호()를 생략할 수 없다.
const arrow = () => {...};
```

- 함수 몸체 정의
    - 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호{}를 생략가능
    - 화살표 함수는 콜백함수로서 정의할 때 유용
    - 화살표 함수는 표현만 간략한 것이만이 아님
    - 화살표 함수는 일반 함수의 기능을 간략화했으며 this도 편리하게 설계

    ```jsx
    //ES5
    [1,2,3].map(function(v){
    	return v*2;
    }
    //ES6
    [1,2,3].map(v => v*2); //[2,4,6]
    ```


(2) 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor
2. 중복된 매개변수 이름을 선언할 수 없음
    - strict mode에서 중복된 매개변수 이름을 선언하면 에러 발생

    ```jsx
    'use strict';
    function normal(a,a){return a + a;}
    //syntaxError: Duplicate parameter name not allowed in this context
    ```

    - 화살표 함수에서도 중복된 매개변수 이름을 선언하면 에러가 발생

    ```jsx
    const arrow = (a, a) => a + a;
    //syntaxError: Duplicate parameter name not allowed in this context
    ```

3. 화살표 함수는 함수 자체의 this, arguments, super, new.target바인딩을 갖지 않음
- 화살표 함수 내부에서 this,arguments,super,new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this,arguments,super,new.target을 참조
- 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this,arguments,super,new.target 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this,arguments,super,new.target을 참조

(3) this

- 화살표 함수가 일반 함수와 구별되는 가장 큰 특징
- 화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많음
- 화살표 함수의 this는 일반 함수의 this와 다르게 동작 → 이는 “콜백 함수 내부의 this 문제”
- 함수를 정의할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정

---

- 화살표 함수는 함수 자체의 this바인딩을 갖지 않음
- 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조
- 이를 lexical this라 함

    → 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위체에 의해 결정된다는 것을 의미

- 메서드를 정의할 때는 ES6메서드 축약 표현으로 정의한 ES6메서드를 사용하는 것이 좋음

```jsx
//Good
const person = {
	name: 'Lee',
	sayHi(){
		console.log(`Hi ${this.name}`);
	}
};
person.sayHi(); //Hi Lee

//Bad
const person = {
	name: 'Lee',
	sayHi: () => console.log(`Hi ${this.name}`)
};
// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역의 this가 가리키는
// 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 반 문자열을 갖는 window.name과 같음
// 전역 객체 window에는 빌트인 프로퍼티 namaeㅣㅇ 존재
person.sayHi(); //Hi
```

(4) super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않음
- 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조

(5) arguments

- 화살표 함수는 함수 자체의 arguments바인딩을 갖지 않음
- 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조
- 화살표 함수로 가변인자 함수를 구현해야 할 때 반드시 Rest 파라미터를 사용

[26.4] Rest 파라미터

(1) 기본 문법

- 매개변수 이름 앞에 세개의 점 . . .을 붙여서 정의한 매개변수를 의미
- 함수에 전달된 인수들의 목록을 배열로 전달받음
- 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당
- 반드시 마지막 파라미터이어야 함
- 단 하나만 선언
- 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length프로퍼티에 영향을 주지 않음

(2) Rest파라미터와 arguments 객체

- 함수와 ES6메서드는 Rest 파라미터와 arguments 객체를 모두 사용
- 화살표 함수는 함수 자체의 arguments 객체를 갖지 않음
- 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest파라미터를 사용해야 함

[26.5] 매개변수 기본값

- 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있음
- Rest 파라미터에는 기본값을 지정할 수 없음
