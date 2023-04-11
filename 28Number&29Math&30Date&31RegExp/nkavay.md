- Number 프로퍼티

Number.EPSILON
1과 1보다 큰 값 중에서 가장 작은 값의 차이. 
부동소수점으로 인한 오차 문제를 해결하기 위해 도입되었다.

Number.MAX_SAFE_INTEGER
자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값을 나타낸다.
자바스크립트에서는 표현할 수 있는 숫자에 한계가 있는데, 
Number 타입 안에서 나타낼 수 있는 가장 큰 수를 가리킨다. 


- Number 메서드

Number.isFinite
유한수인지 판단하는 메서드
빌트인 전역 함수인 isFinite과 차이가 있다. 
isFinite는 인수를 숫자로 암묵적 타입 변환하나, Number.isFinite는 그러지 않는다. 
숫자가 아닌 인수를 받았을 때 Number.isFinite의 반환값은 언제나 false이다.

Number.isSafeInteger
인수가 안전한 정수인지 판단하는 메서드. 
암묵적 타입 변환하지 않는다. 
안전한 수란 IEEE 754 표기 방법으로 나타낼 수 있는 숫자.



- Math 메서드

Math.random
임의의 난수를 반환한다. 0은 포함되지만 1은 포함되지 않는다.

Math.pow
첫번째 인수를 밑으로, 두번째 인수를 지수로 두어서 거듭제곱한 결과를 반환한다.



- Date 메서드

Date.prototype.getTime
1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

Date.prototype.setTime
1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 생성한다.

Date.prototype.getDay
Date 객체의 요일을 숫자로 정했는데, 일요일을 0으로 두고 시작한다.



정규표현식이란?
정규표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
문자열을 대상으로 패턴 매칭 기능을 제공한다.

- RegExp 메서드

RegExp.prototype.exec
문자열 인수의 정규표현식 패턴을 검색해서 매칭 결과를 배열로 반환한다. 
매칭 결과가 없으면 null을 반환한다. g플래그를 지정해도 첫번째 매칭 결과만 반환한다.

RegExp.prototype.test
문자열 인수의 정규표현식 패턴 매칭 결과를 불리언 값으로 반환한다.

RegExp.prototype.match
앞선 exec 메서드와 동일하게, 문자열 인수의 정규표현식 패턴을 검색해서 매칭 결과를 배열로 반환한다. 
exec 메서드와 다른 점은 g플래그를 지정하면 모든 매칭 결과를 반환하는 것이다.

