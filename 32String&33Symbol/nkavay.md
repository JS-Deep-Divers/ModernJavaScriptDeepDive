- String 생성자 함수
String 객체는 생성자 함수: new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있음
String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.
String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

String 래퍼 객체는 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.
단, 문자열은 원시 값이므로 변경할 수 없다. (그리고 이때 에러가 발생하지 않는다.)

String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면, 인수를 문자열로 강제 변환함.

new 연산자를 쓰지않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다.

-String 메서드
String.prototype.indexOf
String.prototype.search
String.prototype.includes
String.prototype.startsWith
String.prototype.endsWith
String.prototype.charAt
String.prototype.substring
String.prototype.slice
String.prototype.toUpperCase
String.prototype.toLowerCase
String.prototype.trim
String.prototype.repeat
String.prototype.replace
String.prototype.split

- Symbol 함수
심벌 값은 Symbol 함수를 호출하여 생성해야 합니다.
Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있는데, 이는 디버깅 용도로만 사용되며 심벌 값 생성에 어떠한 영향도 주지 않습니다.
심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성합니다.
심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않지만 불리언 타입으로는 변환됩니다.

- Symbol.for / Symbol.keyFor 메서드
Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 해당 키와 일치하는 심벌 값을 검색합니다.
Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있습니다.


