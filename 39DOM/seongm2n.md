### 39

- DOM : HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조

[39.1] 노드

(1) HTML 요소와 노드 객체

- 요소노드 , 어트리뷰트(속성) 노드 , 텍스트 노드

<트리 자료구조>

- 비선형 자료구조 : 부모-자식
    - 루트 노드: 0개 이상의 자식 노드 , 최상위
    - 리프 노드: 자식 노드가 없는 노드

(2) 노드 객체의 타입

문서노드

|

요소노드 - 어트리뷰트 노드

|

텍스트토드

<문서노드(document)>

- 루트노드 - docuement객체
- 문서 노드는 window.document 또는 document로 참조
- DOM트리의 노드들에 접근하기 위한 진입점 역할
- 요소,어트리뷰트,텍스트 노드에 접근하려면 문서 노드를 통해야함

<요소노드(element)>

- 문서의 구조 표현

<어트리뷰트 노드(attribute)>

- 속성노드
- 부모 노드 아님
- 요소 노드에만 연결
- 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근

<텍스트 노드(text)>

- 자식노드 아님 → 리프노드
- 문서의 정보 표현
- 요소 노드의 자식 노드
- DOM 트리의 최종단
- 부모 노드인 요소 노드에 접근

(3) 노드 객체의 상속 구조

- 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체
- input요소 노드 객체는 프로트타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있음

| input요소 노드 객체의 특성 | 프로토타입을 제공하는 객체 |
| --- | --- |
| 객체 | Object |
| 이벤트를 발생시키는 객체 | EventTarget |
| 트리 자료구조의 노드 객체 | Node |
| 브라우저가 렌더링 할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element |
| 웹 문서의 요소 중에서 HTML요소를 표현하는 객체 | HTMLElement |
| HTML요소 중에서 input요소를 표현하는 객체 | HTMLInputElement |
- 노드 관련 기능은 Node인터페이스가 제공
- 노드 객체는 상속을 통해 마치 자신의 프로퍼티와 메서드처럼 DOM API 사용

[39.2] 요소 노드 취득

(1) id를 이용한 요소 노드 취득

- id 값을 갖는 HTML요소가 존재하지 않는 경우 getElementById메서드는 null을 반환

(2) 태그 이름을 이용한 요소 노드 취득

- getElementsByTagName메서드 → 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환
- 여러 개의 요소 노드 객체를 갖는 DOM컬렉션 객체인 HTMLCollection객체를 반환

(5) 특정 요소 노드를 취득할 수 있는지 확인

- Element.prototype.matches메서드는 인수로 전달한 CSS선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인
    - 이벤트 위임을 사용할 때 유용

```html
<!DOCTYPE html>
<html>
	<body>
		<ul id="fruits">
				<li class="apple">Apple</li>
				<li class="banana">Banana</li>
				<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		const $apple = document.querySelector('.apple');

		// $apple노드는 '#fruits > li.apple'로 취득
		console.log($apple.matches('#fruits > li.apple')); //true

		// $apple노드는 '#fruits > li.banana'로 취득할 수 없음
		console.log($apple.matches('#fruits > li.banana')); //false
	</script>
</html>

```

(6) HTMLCollection과 NodeList

- HTMLCollection과 NodeList는 모두 유사 배열 객체이면서 이터러블
- 실시간으로 반영하는 살아있는 객체

| HTMLCollection | NodeList |
| --- | --- |
| 언제나 live | non-live |
|  | 경우에 따라 live객체로 동작 |

<HTMLCollection>

- 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거하기 때문에 노드 객체의 상태를 변경해야 할 때 주의
- 이 문제는 해결방법
    - for문을 역방향으로 순회하는 방법으로 회피
    - while문을 사용하여 HTMLCollection객체에 노드 객체가 남아있지 않을 때까지 무한 반복하는 방법으로 회피
    - 부작용을 발생시키는 원인인 HTMLCollection객체를 사용하지 않는 것
    - 유용한 배열의 고차함수(forEach,map,filter,reduce등)을 사용

<NodeList>

- HTMLCollection객체의 부작용 해결
    - querySelectorAll메서드 사용
    - NodeList.prototype은 forEach외에도 item,entries,keys,values메서드 제공
    - 노드 객체의 상태 변경과 상관없이 안전하게 DOM컬렉션을 사용하려면 HTMLCollection이나 NodeList객체를 배열로 변환하여 사용하는 것을 권장
    - 스프레드문법이나 Array.from메서드 사용으로 간단히 배열 변환

[39.3] 노드 탐색

- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티
- setter없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티

(1) 공백 텍스트 노드

- 스페이스, 탭, 줄바꿈(개행)등의 공백 문자는 텍스트 노드 생성

[39.4] 노드 정복 취득

- Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환
- Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환
- Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9 반환

[39.5] 요소 노드의 텍스트 조작

(1) nodeValue

- 노드 탐색, 노드 정보 프로퍼티는 모두 읽기 전용 접근자 프로퍼티
- nodeValue프로퍼티는 setter와 getter모두 존재하는 접근자 프로퍼티
- nodeValue프로퍼티는 참조와 할당 모두 가능
- 텍스트노드가 아닌 노드, 문서 노드나 요소 노드의 nodeValue프로퍼티를 참조하면 null반환



(2) textContent

- 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경
- childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값, 텍스트를 모두 반환
- 이때 HTML마크업은 무시
-

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello <span>world!</span></div>
	</body>
	<script>
		//#foo 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가
		//이때 HTML마크업이 파싱되지 않음
		document.getElementById('foo').textContent = 'Hi <span> there!</span>';
	</script>
</html>
```

- 브라우저에 Hi <span> there!</span> 그대로 출력
- textContent프로퍼티와 유사한 동작을 하는 innerText프로퍼티가 있음
- innerText 프로퍼티는 사용하지 않는 것이 좋음
    - innerText프로퍼티는 CSS에 순종적
    - innerText 프로퍼티는 CSS에 의해 비표시(visibility:hidden;)로 지정된 요소 노드의 텍스트를 반환하지 않음
    - innerText 프로퍼티는 CSS를 고려해야 하므로 textContent프로퍼티보다 느림

[39.6] DOM조작

- 새로운 노드를 생성하여 DOM에 추가하거나 기존노드를 삭제 또는 교체
- 리플로우와 리페인트 발생

(1) innerHTML

- HTML 마크업을 취득하거나 변경
- HTML마크업이 포함된 문자열을 그대로 반환

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello <span>world!</span></div>
	</body>
	<script>
		//HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영
		document.getElementById('foo').innerText = 'Hi <span> there!</span>';
	</script>
</html>
```

- 브라우저에 Hi there!로 출력
- HTML 마크업 문자열로 간단히 DOM조작 가능
- 주의할점!
    - 사용자로부터 입력받은 데이터를 그대로 innerHTML프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격(XSS)에 취약하므로 위험
    - HTML 마크업 내에 자바스크립트 악성 코드가 포함되어 있다면 파싱 과정에서 그대로 실행될 가능성이 있음
- 장점 : innerHTML 프로퍼티를 사용한 DOM조작은 구현이 간단하고 직관적
- 단점
    - 크로스 사이트 스크립팅 공격에 취약
    - 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML마크업 문자열을 파싱하여 DOM을 변경

(5) 노드 삽입

<마지막 노드로 추가>

- Node.prototype.appendChild

<지정한 위치에 노드 삽입>

- Node.prototype.insertBefore(newNode, childNode)
    - 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입

(7) 노드 복사

- Node.prototype.cloneNode([deep:true | false])
