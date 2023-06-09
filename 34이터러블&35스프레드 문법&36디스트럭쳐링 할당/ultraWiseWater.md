### 34. 이터러블

## 이터레이션 프로토콜

- 이터레이션 프로토콜은 순회 간으한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

- 이터레이션 프로토콜에는 `이터러블 프로토콜`과 `이터레이터 프로토콜`이 있다.

**어터러블 프로토콜**

- 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.

- 이터러블은 for...of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

**이터레이터 프로토콜**

- 이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.

- 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

### 35. 스프레드 문법

- 스프레드 문법 ... 은 하나로 뭉쳐있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목룍으로 만든다.

- 스크레드 문법을 사용할 수 있는 대상:
  Array, String, Map, Set, DOM 컬렉션

### 36. 디스트럭처링 할당

- 디스트럭팅 할당은 구조화된 배열/이터러블/객체를 destructing하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

## 배열 디스트럭처링 할당

- 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다.

## 객체 디스트럭처링 할당

- 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.

- 객체 디스트럭처링 할당의 대상은 객체이어야하며, 할당 기준은 프로퍼티 키다.

- 순서는 의미가 없으며 선언된 변수 이르과 프로퍼티 키가 일치하면 할당된다.
