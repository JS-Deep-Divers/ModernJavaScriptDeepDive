# 자바스크립트 딥 다이브_10 객체 리터럴

## 기억하고 싶은 내용

### 객체

- 변경 불가능한 **원시 타입의 값**과 달리, <u>**객체는 변경 가능한 값이다.**</u>

- 객체는 프로퍼티의 집합이다.
  - 프로퍼티는 `key`와 `value`로 구성된다
- 프로퍼티 값이 함수인 경우 **메서드**라고 불린다.
- 자바스크립트에서의 객체는 함수와 밀접한 관계를 가진다. 함수로 객체를 생성하기도 하며 함수자체가 객체이기도 하다.



### 객체 리터럴에 의한 객체 생성

**인스턴스**

- 클래스에 의해 생성되어 메모리에 저장된 실체를 말한다. <u>객체지향 프로그래밍에서 객체는 클래스와 인스턴스를 포함한 개념이다.</u>



**자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 객체 생성 방법을 지원한다**

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.creat 메서드
- 클래스(ES6)



**객체 리터럴**

- 객체 리터럴은 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식이다. 객체를 생성하기 위해 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요가 없다.



### 프로퍼티

- 객체는 프로퍼티의 집합이먀, 프로퍼티는 `key`와 `value`로 구성된다.
  - `key` : 빈 문자열을 포함하는 모든 문자열 또는 심벌값
    - 일반적으로 문자열을 사용한다.
  - `value` : 자바스크립트에서 사용할 수 있는 모든 값 



**프로퍼티의 `Key`**

- 식별자 네이밍 규칙을 준수하는 것을 권장한다.

  - 식별자 네이밍 규칙을 준수하지 않을 경우 `'last-name' `과 같이 따옴표를 사용한다. 

- 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있다.

  - 이 경우 프로퍼티 키로 사용할 표현식을 대괄호`[]`로 묶어야 한다.

    ```javascript
    let obj = {};
    let key = "hey";
    
    // #ES5
    obj[key] ='world';
    
    // #ES6
    let new_obj = {[key] : 'world'};
    ```



### 프로퍼티에서 특히 기억 할 내용

- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다. <u>이때 에러는 발생하지 않는다.</u>
- 객체에 존재하지 않는 프로퍼티에 접근하면 `undefined`를 반환한다. <u>이때 `ReferenceError`가 발생하지 않는다</u>
- 만약 존재하지 않는 프로퍼티를 삭제하면 <u>아무런 에러 없이 무시된다.</u>



### ES5 ES6 추가된 내용

**[ES6] 프로퍼티 Key 생략 기능**

프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 대 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수이름으로 자동생성된다.

```javascript
let x, y =2;
const obj = {x, y}; // {x: 1, y: 2};
```



**계산된 프로퍼티 이름**

프로퍼티 Key를 계산된 값으로 동적생성

```javascript
const obj = "prop";
let i = 0;
obj[prefix + '-' + ++i] = i; // [ES5]

const obj2 = {
  [`${prefix}-${++i}` : 1] // [ES6]
}
```



**[ES6] 메서드 축약 표현**

```javascript
const obj = {
  name: 'Jung',
  sayHi() {
    console.log("Hi "+ this.name);
  }
}
```

- 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작한다.



## 느낀점

- 몰랐던 추가적인 기능들이나 생각 외 로 동작하는 기능들이 많았다.
- 예외적인 부분은 코드를 짤 때에 결과를 예측할 수 있어야 하는데 간과하기 쉬운 내용이어서 기억해두고 싶다.
- C언어를 그래도 알고 봐서 더 이해가 되는 느낌이었다. 이 책은 자바스크립트 엔진이 어떻게 해석하는지에 대한 깊은 이야기 까지 다뤄서 그런것 같다.
- 아쉬운 것은 결국 엔진을 만든 회사에 따라 정확한 동작까지는 알 수 없다는 점이다.
- 자바스크립트는 참 오묘한 맛이다. 근데 달아..





# 자바스크립트 딥 다이브_11 원시 값과 객체의 비교

- `원시` 값을 변수에 할당하면 변수(확보된 메모리 공간)에는 실제 값이 저장된다.
  - 원시 값을 갖는 변수에 다른 변수를 할당하면 **원본의 원시값이 복사**되어 전달된다.
  - 원시값은 변경 불가능한 값이다. 때문에 데이터의 신뢰성을 보장한다.
  - 변수에 원시값을 할당하고 새로운 값으로 재할당 된 경우 변수는 참조하던 메모리의 주소를 변경할 뿐 원시값이 바뀌지 않는다.
- `객체`를 변수에 할당하면 변수(확보된 메모리 공간)에는 참조 값이 저장된다.
  - 객체를 가리키는 변수를 다른 변수에 할당하면 **원본의 참조 값이 복사**되어 전달된다.
  - 객체는 크기가 일정하지 않기 때문에, 복사를 하게된다면 생성 비용이 많이든다. 이 비용을 절약하여 성능을 향상시키기 위해 **객체는 변경 가능한 값** 으로 설계되어 있다.
  - 객체는 **여러개의 식별자가 하나의 객체를 공유할 수 있다**



### 느낀점

객체의 프로퍼티를 사용할 일이 많아서인지, 프로퍼티쪽에 더 기억하고 싶은 내용이 많았다.

프로퍼티의 예외적인 부분도 참.. 많다..😂. 이 부분도 유의하도록 의식해야겠다.

11장은 c언어에서의 포인터의 개념이 있어서인지 생각보다 편하게 받아드렸다. 책 자체의 내용에서 그림을 그려가면서 보여주기 때문에 좋았던거 같다.