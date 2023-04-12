# 자바스크립트 딥다이브 26.함수의 추가 기능

## 기억하고 싶은 내용

### 메서드

- ES6 사양에서 메서드는 축약 표현으로 정의된 함수만을 의미한다.

   ```javascript
   const obj = {
     x: 1,
     foo() {return this.x}, // 메서드
     bar: function() {return this.x}  // 일반 함수
   }
   ```



### 화살표 함수

화살표 함수는 콜백 함수 내부에서 `this`가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

- 일반 함수는 중복된 매개변수 이름을 선언할 수 있으나, 화살표 함수에서는 중복된 매개변수를 선언할 수 없다.
- 화살표 함수는 자체의 `this`,`arguments`, `super`, `new.target`을 갖지 않기 때문에 스코프 체인을 통해 상위 스코프의 `this`, `arguments`, `super`,`new.target`을 참조한다.
- 프로퍼티에 할당한 화살표 함수도 스코체인 상에서 <u>가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 `this`를 참조한다.</u>



### arguments

- 화살표 함수는 함수 자체의 `arguments`바인딩을 갖지 않는다. 따라서 **화살표 함수 내부에서 `argument` 를 참조하면 `this`와 마찬가지로 상위 스코프의 argument를 참조한다.**

- 매개변수의 기본값은 arguments 객체에 아무런 영향을 주지 않는다.

  ```javascript
  function sum(x, y = 0){
    console.log(arguments);
  }
  
  console.log(sum.length) // 1
  
  sum(1); // Arguments {'0': 1}
  sum(1, 2); // Arguments {'0': 1, '1': 2}
  ```

  



### Rest 파라미터

- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
- Rest 파라미터는 단 하나만 선언할 수 있다.



### arguments 객체와 Rest 파라미터

- 둘 다 인수들의 정보를 갖고 있다는 점에서 동일하다
- 하지만, **arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용하려면 객체를 배열로 변환해야 하는 번거로움이 있다.**







## 느낀점

- 어렴풋이 알고 있던 내용을 되새기는 시간이었다.
- 여전히 프로토타입 관련된 부분에 있어서는 어려움이 있다.