## 23장 실행 컨텍스트 ,13장 스코프

### [1]  함수 스코프(function scope) - __(1)__, 블록 스코프(block scope) - __(2)__
### 아래 보기를 보고 고르시오.(중복 가능)
#### <보기> let, var, const
<details>
<summary>답</summary>
<div markdown="1">
(1) var (2) let,const   <p386>
</div>
</details>



### [2] 전역코드와 함수 코드로 이루어진 소스코드에서 자바스크립트 엔진은 먼저 전역코드를 평가하여 전역 실행 컨텍스트를 생성한다. 그리고 함수가 호출되면 함수 코드를 평가하여 함수 실행 컨텍스트를 생성한다.  여기서  생성된 실행 컨텍스트는 무슨 자료구조로 관리되는가?
### 아래 보기를 보고 고르시오.
#### <보기> (1)스택 (2)큐
<details>
<summary>답</summary>
<div markdown="1">
(1)스택  <p365>
</div>
</details>



### [3] 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트는?
### 아래 보기를 보고 고르시오.
#### <보기> (1)variable 환경 (2)Lexical 환경
<details>
<summary>답</summary>
<div markdown="1">
(2)Lexical환경 <p366>
</div>
</details>
