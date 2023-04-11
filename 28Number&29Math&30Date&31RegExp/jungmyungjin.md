# 2023.03.31 자바스크립트 딥다이브 (28 Number/29 Math/30 Data/31 RegExp)

## 기억하고 싶은 내용

### Date

**new Date()**

- new 연산자와 함께 호출하면 <u>현재 날짜와 시간을 가지는 Date 객체를 반환</u>한다.
- `Date 객체`를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다
- Date생성자 함수를 <u>new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.</u>
- new Date 생성 시 **month에는 +1**을 해주어야한다.(1월 -> 0)



### RegExp

**생성 방법**

- 리터럴을 사용한 생성 : `const regexp = /is/i`

- `RegExp 생성자 함수`를 사용하여 생성 : `const regexp = new RegExp(/is/i);`

  - 변수를 사용한 동적 RegExp 객체를 생성할 수 있다.

    ```javascript
    const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;
    count('Is this all there is?', 'is') // -> 3
    count('Is this all there is?', 'xx') // -> 0
    ```



### RegExp메서드

**exec 메서드**

- 인수로 전달받은 문자열에 대해 정규식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.
- 매칭 결과가 없는 경우 null을 반환한다.



**test 메서드**

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.



**match 메서드**

- String 표준 빌트인 객체가 제공하는 match 메서드는 대상문자열과 **인수로 전달받은 정규표현식**과의 매칭을 배열로 반환한다.

- <u>`exec` 메서드는 **첫번째 결과**만 반환</u>하지만, <u>`String.prototype.match` 메서드는 **모든 매칭 결과**를 배열로 반환</u>한다.

  



## 느낀점

- 이미 써본 객체와 매서드가 많아서 편하게 전체적으로 훑어본것 같다.
- 그 중 쓸 때마다 헷깔렸던 Date함수는 전체적으로 훑어보면서 정리가 되어 도움이 된 것 같다.
- RegExp는 정규표현식에 익숙하여 크게 어렵지는 않았다. 다만 지원하는 메서드나 생성하는 방법에 대해서는 제대로 몰랐는데, 도움이 된 것 같다.



