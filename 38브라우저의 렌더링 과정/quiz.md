### [1]

자바스크립트 파싱에 의한 DOM생성이 중단되는 문제를 해결하기 위해 HTML5부터 script 태그에 async와 ____ 어트리뷰트가 추가되었다.

____에 들어갈 단어는?

|—부연설명—|

- async와 ____단어는 src어트리뷰트를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용한다.
- ____은 DOM 생성이 완료된 이후 실행되어야할 자바스크립트에 유용하다.
- async와 ____은 HTML파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다.

<details>
<summary>답</summary>
<div markdown="1">

defer p.675

</div>
</details>

### [2]

렌더링 엔진이 DOM을 생성해 나가다가 CSS를 로드하는 link태그나 style 태그를 만나게 된다면?

(1) CSSOM을 생성 (2) 일단 정지 (3) script태그를 찾음

<details>
<summary>답</summary>
<div markdown="1">

(2) 일단 정지 p.667

</div>
</details>

### [3]

브라우저의 렌더링 과정이 다음 보기와 같이 반복해서 실행될 시 레이아웃 계산과 페인팅이 재차 실행된다.

이는 리렌더링 비용이 __1__ 들고, 성능에 __2__주는 작업

|—보기—|

- 자바스크립트에 의한 노드 추가 또는 삭제
- 브라우저 창의 리사이징에 의한 뷰포트 크기 변경
- HTML요소의 레이아웃에 변경을 발생시키는 width/height,margin,padding,border,display,position,to/right/bottom/left등의 스타일 변경

<details>
<summary>답</summary>
<div markdown="1">

1. 많이
2. 악영향/나쁜영향 p.669

</div>
</details>
