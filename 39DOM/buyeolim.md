# 39장. DOM

브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM 생성  
DOM(Document Object Model)은 HTML문서의 계층적 구조와 정보를 표현  
이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조

## 39.1 노드

### 39.1.1 HTML 요소와 노드 객체

HTML 요소는 HTML 문서를 구성하는 개별적인 요소

```html
<div class="greeting">Hello</div>

<!-- HTML 요소의 구조 -->
<[start_tag] [attribute_name]="[attribute_value]">[contents]</[end_tag]>

<!-- HTML 요소와 노드 객체
  요소 노드   div - class="greeting" 어트리뷰트 노드
             |
  텍스트 노드 "Hello"
-->
```

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환

- HTML 요소의 어트리뷰트 &rarr; 어트리뷰트 노드로 변환
- HTML 요소의 텍스트 콘텐츠 &rarr; 텍스트 노드로 반환

HTML 문서는 HTML 요소들의 집합으로 이루어짐  
HTML 요소는 중첩 관계를 가짐  
HTML 요소간에는 중첩 관계에 의해 계층적인 부자 관계 형성  
HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성

**트리 구조**  
트리 자료구조는 노드들의 계층 구조  
부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조(부자, 형제 관계)를 표현하는 비선형 자료구조  
트리 자료구조는 하나의 최상위 노드에서 시작

- 루트 노드: 부모가 없는 최상위 노드, 0개 이상의 자식 노드를 보유
- 리프 노드: 자식 노드가 없는 노드

**DOM(DOM 트리)**: 노드 객체들로 구성된 트리 자료구조, 노드 객체의 트리로 구조화

### 39.1.2 노드 객체의 타입

DOM은 노드 객체의 계층적인 구조로 구성  
노드 객체는 종류가 있고 상속 구조를 가짐  
노드 객체는 총 12개 종류(노드 타입)이며, 중요한 노드 타입은 다음 4가지

#### 문서 노드

문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴  
document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체  
HTML 문서당 document 객체는 유일  
문서 노드(document 객체)는 DOM 트리의 루트 노드이므로 DOM 트리의 노드에 접근하기 위한 진입점(요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 함)

#### 요소 노드

요소 노드는 HTML 요소를 가리키는 객체  
요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이를 통해 정보를 구조화  
요소 노드는 문서의 구조를 표현

#### 어트리뷰트 노드

어트리뷰트 노드는 HTML 요소를 가리키는 객체  
어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결  
요소 노드는 부모 노드와 연결되어 있지만, 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결  
어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근 필요

#### 텍스트 노드

텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체  
요소 노드가 문서의 구조를 표현한다면 텍스트 노드는 문서의 정보를 표현  
텍스트 노드는 요소 노드의 자식 노드, 자식 노드를 가질 수 없는 리프 노드  
텍스트 노드는 DOM 트리의 최종단  
텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근 필요

### 39.1.3 노드 객체의 상속 구조

DOM은 HTML 문서의 계층적 구조와 정보를 표현, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조  
DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용 가능  
이를 통해 노드 객체는 자신의 부모, 형제, 자식 탐색 및 자신의 어트리뷰트와 텍스트 조작 가능

DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체

노드 객체의 기능 두 가지 분류  
&rarr; 노드 객체의 종류(노드 타입)에 상관없이 모든 노드 객체가 공통으로 갖는 기능  
&rarr; 노드 타입에 따른 고유한 기능

DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공  
DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작 가능

## 39.2 요소 노드 취득

HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드 취득 필요  
요소 노드의 취득은 HTML 요소를 조작하는 시작점, 이를 위해 DOM은 요소 노드를 취득할 수 있는 다양한 메서드 제공

### 39.2.1 id를 이용한 요소 노드 취득

Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환  
getElementById 메서드는 Document.prototype의 프로퍼티, 반드시 문서 노드인 document를 통해 호출 필요  
getElementById 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환

### 39.2.2 태그 이름을 이용한 요소 노드 취득

Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소들을 탐색하여 반환  
해당 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환  
HTMLCollection 객체는 유사 배열 객체이면서 이터러블

### 39.2.3 class를 이용한 요소 노드 취득

Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달 한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환  
인수로 전달할 class 값은 공백으로 구분하여 어러 개의 class를 지정  
해당 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환

### 39.2.4 CSS 선택자를 이용한 요소 노드 취득

CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법  
Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환

- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환
- 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null을 반환
- 인수로 전달한 CSS 선택자가 문법에 맞지 ㅇ낳는 경우 DOMException 에러가 발생

Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소를 탐색하여 반환  
해당 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환  
NodeList 객체는 유사 배열 객체이면서 이터러블

- 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 NodeList 객체를 반환
- 인수로 전달한 CSS 선택자 문법에 맞지 않는 경우 DOMException 에러가 발생

### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인

Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인  
해당 메서드는 이벤트 위임을 사용할 때 유용

### 39.2.6 HTMLCollection과 NodeList

DOM 컬렉션 객체인 HTMLCollection과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체  
HTMLCollection과 NodeList는 모두 유사 배열 객체이면서 이터러블

HTMLCollection과 NodeList의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체  
&rarr; HTMLCollection은 언제나 live 객체로 동작  
&rarr; NodeList는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작, 경우에 따라 live 객체로 동작

#### HTMLCollection

getElementsByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체  
따라서 HTMLCollection 객체를 살아 있는(live) 객체라고 부름

HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 for 문으로 순회하면서 노드 객체 상태를 변경해야 할 때 주의  
이 문제는 for 문을 역방향으로 순회하는 방법으로 회피 가능, 더 간단한 해결책은 HTMLCollection 객체를 사용하지 않고 유용한 배열의 고차 함수(forEach, map, filter, reduce 등)를 사용

#### NodeList

HTMLCollection 객체의 부작용을 해결하기 위해 getElementsByTagName, getElementsByClassName 메서드 대신 querySelectorAll 메서드를 사용하는 방법 존재  
querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환  
이때 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는(non-live) 객체

NodeList 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체를 동작  
childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의 필요

이처럼 HTMLCollection, NodeList 객체는 예상과 다르게 동작할 때가 있어 주의 필요  
노드 객체의 상태의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 두 객체를 배열로 변환하여 사용하는 것을 권장  
배열로 변환하면 배열의 유용한 고차 함수(forEach, map, filter, reduce 등)를 사용 가능  
두 객체는 모두 유사 배열 객체이면서 이터러블  
따라서 스프레드 문법, Array.from 메서드를 사용하여 간단히 배열로 변환 가능

## 39.3 노드 탐색

요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색할 필요성 존재  
DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공  
노드 탐색 프로퍼티는 모두 접근자 프로퍼티(단, setter 없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티)

### 39.3.1 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드(공백 텍스트 노드)를 생성  
노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의

### 39.3.2 자식 노드 탐색

자식 노드를 탐색하기 위해 다음의 노드 탐색 프로퍼티 사용

- **Node.prototype.childNodes**  
  자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환  
  childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있음
- **Element.prototype.children**  
  자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환  
  children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않음
- **Node.prototype.firstChild**  
  첫 번째 자식 노드를 반환  
  firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드
- **Node.prototype.lastChild**  
  마지막 자식 노드를 반환  
  lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드
- **Element.prototype.firstElementChild**  
  첫 번째 자식 요소 노드를 반환  
  firstElementChild 프로퍼티는 요소 노드만 반환
- **Element.prototype.lastElementChild**  
  마지막 자식 요소 노드를 반환  
  lastElementChild 프로퍼티는 요소 노드만 반환

### 39.3.3 자식 노드 존재 확인

자식 노드가 존재하는지 확인하려면 Node.prototype.hasChildNodes 메서드를 사용  
해당 메서드는 자식 노드가 존재하면 true, 존재하지 않으면 false 반환  
해당 메서드는 텍스트 노드를 포함하여 자식 노드의 존재를 확인

자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 hasChildNodes 메서드 대신 children.length 또는 Element 인터페이스의 childElementCount 프로퍼티를 사용

### 39.3.4 요소 노드의 텍스트 노드 탐색

요소 노드의 텍스트 노드는 요소 노드의 자식 노드  
따라서 firstChild 프로퍼티로 접근 가능, 해당 프로퍼티는 첫 번째 자식 노드를 반환  
반환한 노드는 텍스트 노드 또는 요소 노드

### 39.3.5 부모 노드 탐색

부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용  
텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없음

### 39.3.6 형제 노드 탐색

부모 노드가 같은 형제 노드를 탐색하려면 다음의 노드 탐색 프로퍼티 사용

- **Node.prototype.previousSibling**  
  부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환  
  previousSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있음
- **Node.prototype.nextSibling**  
  부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환  
  nextSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있음
- **Element.prototype.previousElementSibling**  
  부모 노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환  
  previousElementSibling 프로퍼티는 요소 노드만 반환
- **Element.prototype.nextElementSibling**  
  부모 노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환  
  nextElementSibling 프로퍼티는 요소 노드만 반환

## 39.4 노드 정보 취득

노드 객체에 대한 정보를 취득하려면 다음의 노드 정보 프로퍼티를 사용

- **Node.prototype.nodeType**  
  노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환  
  노드 타입의 상수는 Node에 정의되어 있음
  - Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1 반환
  - Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3 반환
  - Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9 반환
- **Node.prototype.nodeName**  
  노드의 이름을 문자열로 반환
  - 요소 노드: 대문자 문자열로 태그 이름("UL", "LI" 등)을 반환
  - 텍스트 노드: 문자열 "#text"를 반환
  - 문서 노드: 문자열 "#document"를 반환

## 39.5 요소 노드의 텍스트 조작

### 39.5.1 nodeValue

Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티  
nodeValue 프로퍼티는 참조와 할당 모두 가능  
노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환

### 39.5.2 textContent

Node.prototype.texContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경  
요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환  
이때 HTML 마크업은 무시

참고. textContent 프로퍼티와 유사한 동작을 하는 innerText 프로퍼티 사용하지 않는 것이 좋음

- innerText 프로퍼티는 CSS에 순종적, CSS에 의해 비표시로 지정된 요소 노드의 텍스트를 반환하지 않음
- innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느림

## 39.6 DOM 조작

DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것  
DOM 조작에 의해 DOM에 새로운 노드가 추가, 삭제되면 리플로우와 리페인트 발생하는 원인이 되므로 성능에 영향을 줌

### 39.6.1 innerHTML

Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티  
요소 노드의 HTML 마크업을 취득하거나 변경  
innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환  
해당 프로퍼티를 사용하면 HTML 마크업 문자열로 간단히 DOM 조작이 가능  
입력 받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약

### 39.6.2 insertAdjacentHTML 메서드

Element.prototype.insertAdjacentHTML(position, DOMString) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입  
insertAdjacentHTML 메서드 innerHTML 프로퍼티보다 효율적이고 빠름  
innerHTML 메서드와 동일하게 크로스 사이트 스크립팅 공격에 취약

### 39.6.3 노드 생성과 추가

innerHTML, insertAdjacentHTML 메서드는 HTML 마크업 문자열을 파싱하여 노드를 생성하고 DOM에 반영  
DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드 제공

#### 요소 노드 생성

Document.prototype.createElement(tagName) 메서드는 요소 노드를 생성하여 반환  
createElement 메서드의 매개변수 tagNage에는 태그 이름을 나타내는 문자열을 인수로 전달  
createElement 메서드로 생성한 요소 노드를 생성할 뿐 DOM에 추가하지는 않음  
이후에 생성된 요소 노드를 DOM에 추가하는 처리가 별도로 필요

#### 텍스트 노드 생성

Document.prototype.appendChild(childNode) 메서드는 매개변수 childNode에게 인수로 전달한 노드를 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가

#### 요소 노드를 DOM에 추가

Node.prototype.appendChild 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를, 목적으로한 요소 노드의 마지막 자식의 요소로 추가  
하나의 요소 노드를 생성하여 DOM 한번 추가하면 DOM은 한 번 변경(리플로우, 리페인트 실행)

### 39.6.4 복수의 노드 생성과 추가

여러 개의 요소 노드를 생성하여 DOM에 추가하면, 해당 요소 노드를 추가하는 만큼 리플로우와 리페인트 실행  
DOM을 변경하는 것은 높은 비용이 드는 처리, 따라서 가급적 횟수를 줄이는 편이 성능에 유리

DOM을 여러 번 변경하는 문제를 회피하기 위해 컨테이너 요소를 미리 생성하여 그 자식 노드로 추가하여, 목적으로한 요소에 자식으로 추가한다면 DOM은 한 번만 변경  
이는 성능에 유리하나 불필요한 컨테이너 요소가 DOM에 추가되는 부작용 발생

이 문제는 DocumentFragment 노드를 통해 해결 가능  
해당 노드는 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용  
기존 DOM과는 별도로 존재하므로 DocumentFragment 노드에 자식 노드를 추가하여도 기존 DOM에는 어떠한 변경도 발생하지 않음  
또한 DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가  
DOM 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행

### 39.6.5 노드 삽입

#### 마지막 노드로 추가

Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가  
노드를 추가할 위치를 지정할 수 없고 언제나 마지막 자식 노드로 추가

#### 지정한 위치에 노드 삽입

Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입  
두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드

## 39.6.6 노드 이동

DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가(노드 이동)

### 39.6.7 노드 복사

Node.prototype.cloneNode(\[deep: true | false]) 메서드는 노드의 사본을 생성하여 반환

매개변수 deep  
&rarr; true: 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성  
&rarr; false 또는 생략: 노드를 얕은 복사하여 노드 자신만의 사본을 생성

### 39.6.8 노드 교체

Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체  
&rarr; 첫 번째 매개변수 newChild: 교체할 새로운 노드
&rarr; 두 번째 매개변수 oldChild: 이미 존재하는 교체될 노드를 인수로 전달

### 39.6.9 노드 삭제

Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제  
인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드

## 39.7 어트리뷰트

### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

HTML 문서의 구성 요소인 HTML 요소는 여러 개의 어트리뷰트(속성)를 보유 가능  
HTML 어트리뷰트는 HTML 요소의 시작 태그에 `어트리뷰터 이름="어트리뷰트 값"` 형식으로 정의

글로벌 어트리뷰트: id, class, style, title, lang, tabindex, draggable, hidden 등  
이벤트 핸들러 어트리뷰트: onclick, onchange, onfocus, onblur, oninput, onkeypress, onkeydown, onkeyup, onmouseover, onsubmit, onload 등

글로벌 및 이벤트 핸들러 어트리뷰트는 모든 HTML 요소에서 공통적으로 사용 가능  
특정 HTML 요소에만 한정적으로 사용 가능한 어트리뷰트도 존재

요소 노드의 모든 어트리뷰트 노드는 요소 노드의 Element,prototype,attributes 프로퍼티로 취득 가능  
attributes 프로퍼티는 getter만 존재하는 읽기 전용 접근자 프로퍼티, 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체 반환

### 39.7.2 HTML 어트리뷰트 조작

Element.prototype.getAttribute/setAttribute 메서드를 사용하면 attributes 프로퍼티를 통하지 않고 요소 노드에서 메서드를 통해 직접 HTML 어트리뷰트 값을 취득하거나 변경 가능

Element.prototype.getAttribute(attributeName) &rarr; HTML 어트리뷰트값 참조
Element.prototype.setAttribute(attributeName, attributeValue) &rarr; HTML 어트리뷰트값 변경

### 39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티

요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(DOM 프로퍼티)가 존재  
DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 보유  
DOM 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티(참조, 변경 가능)

**HTML 어트리뷰트의 역할**  
HTML 요소의 초기 상태, 이는 변하지 않음

#### 어트리뷰트 노드

HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리

#### DOM 프로퍼티

사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리  
DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태 유지

#### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

대부분의 HTML 어트리뷰트는 HTML 어트리뷰트 이름과 동일한 DOM 프로퍼티와 1:1로 대응  
언제나 1:1로 대응하는 것은 아니며, HTML 어트리뷰트 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아님

#### DOM 프로퍼티 값의 타입

getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열  
DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있음

### 39.7.4 data 어트리뷰트와 dataset 프로퍼티

data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터 교환 가능  
data 어트리뷰트의 값은 HTMLElement.dataset 프로퍼티로 취득 가능  
dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트 정보를 제공하는 DOMStringMap 객체를 반환

## 39.8 스타일

### 39.8.1 인라인 스타일 조작

HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경  
style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환

### 39.8.2 클래스 조작

. 으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음, HTML 요소의 class 어트리뷰트 값을 변경하여 HTML 요소의 스타일 변경 가능

#### className

Element.prototype.className 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경

#### classList

Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환

### 39.8.3 요소에 적용되어 있는 CSS 스타일 참조

style 프로퍼티는 인라인 스타일만 반환  
따라서 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 style 프로퍼티로 참조 불가  
HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야 할 경우 getComputedStyle 메서드를 사용

## 39.9 DOM 표준

| 레벨        | 표준 문서 URL                           |
| :---------- | :-------------------------------------- |
| DOM Level 1 | https://www.w3.org/TR/REC-DOM-Level-1/  |
| DOM Level 2 | https://www.w3.org/TR/DOM-Level-2-Core/ |
| DOM Level 3 | https://www.w3.org/TR/DOM-Level-3-Core/ |
| DOM Level 4 | https://dom.spec.whatwg.org/            |
