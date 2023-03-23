# 16. 프로퍼티 어트리뷰트 / 17. 생성자 함수에 의한 객체 생성 - 3문제

## [1] Object.preventExtensions, Object.seal, Object.free

객체 변경 방지에는 다음과 같은 메서드가 있다

- `Object.preventExtensions` : 객체 확장금지
- `Object.seal` : 객체 밀봉
-  `Object.freeze` : 객체 동결

이때, `Object.seal`에서 가능한 것은? (모두 쓰세요)

```javascript
다음 
프로퍼티 추가, 프로퍼티 삭제, 프로퍼티 값 읽기, 프로퍼티 값 쓰기, 프로퍼티 어트리뷰트 재정의
```

<details>
<summary>정답</summary>
  프로퍼티 값 읽기/쓰기








## [2] 생성자 함수란 무엇인가요?

<details>
  <summary> 정답 </summary>
  new 연산자와 함꼐 호출하여 객체 인스턴스를 생성하는 함수를 말한다.

</details>






## [3] 생성자 함수에 의핸 객체 생성 방식의 장점은 무엇인가요?

<details>
<summary>정답</summary>
객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

