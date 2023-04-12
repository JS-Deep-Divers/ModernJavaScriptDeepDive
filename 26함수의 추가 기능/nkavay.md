- 메서드
ES6 이전의 모든 함수는 callable, construcor다. 콜백 함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성한다.
ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.

- 화살표 함수
non-constructor
함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
메서드(일반적인 의미)나 프로토타입 객체의 프로퍼티에 화살표 함수를 할당X
