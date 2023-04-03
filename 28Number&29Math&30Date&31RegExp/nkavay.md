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
