### 39. DOM

- DOM: HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조

## 노드

# HTML 요소와 노드 객체

**트리 자료구조**

- 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 `비선형 자료구조`

- 하나의 최상위 노드에서 시작

- 최상위 노드는 부모 노드가 없으며, `루트 노드`라 한다.

- 루트 노드는 0개 이상의 자식 노드를 갖는다.

- 자식 노드가 없는 노드를 `리프 노드`라 한다.

# 노드 객체의 타입

- 문서 노드:
  DOM 트리의 최상위에 존재하는 `루트노드`로서 document 객체를 가리킨다, window.document 또는 document로 참조

- 요소 노드:
  HTML 요소를 가리키는 객체

- 어트리뷰트 노드:
  HTML 요소의 어트리뷰트를 가리키는 객체, 요소노드와 연결되어 있음, 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 함

- 텍스트 노드:
  HTML 요소의 텍스트를 가리키는 객체, 자식 노드를 가질 수 없는 `리프 노드`, 요소 노드의 자식노드, DOM 트리의 최종단, 텍스트 노드에 접근하려면 부모 노드인 요소 노드에 접근해야함

# 노드 객체의 상속 구조

- 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체

- 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가짐

- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받음

- `문서노드`는 Document, HTMLDocument 인터페이스를 상속받음

- `어트리뷰트 노드`는 Attr, `텍스트 노드`는 CharacterData 인터페이스를 각각 상속받음

- input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있음

**input 요소 노드 객체의 특성**

<input 요소 노드 객체의 특성 : 프로토타입을 제공하는 객체>

겍체: Object

이벤트를 발생시키는 객체: EventTarget

트리 자료구조의 노드 객체: Node

브라우저가 렌더링 할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체: Element

웹 문서의 요소 중에서 HTML 요소를 표현하는 사례: HTMLElement

HTML 요소 중에서 input 요소를 표현하는 사례: HTMLInputElement

- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티나 메서드의 집합인 DOM API로 제공한다.

- DOM API를 통해 HTML 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

# 특정 요소 노드를 취득할 수 있는지 확인

- Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.

- 이벤트 위임을 사용할 떄 유용하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">apple</li>
      <li class="banana">banana</li>
      <li class="orange">orange</li>
    </ul>
  </body>
  <script>
    const $apple = document.querySelector(".apple");

    // $apple 노드는 '#fruits > li.apple'로 취득할 수 있다.
    console.log($apple.matches("#fruits > li.apple")); // true

    // $apple 노드는 '#fruits > li.banana'로 취득할 수 없다.
    console.log($apple.matches("#fruits > li.banana")); // false
  </script>
</html>
```

# HTMLCollection과 NodeList

- HTMLCollection 과 NodeList 모두 유사 배열 객체이면서 이터러블이다.

- for...of 문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단히 배열로 변환할 수 있다.

**HTMLCollection**

- 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체

**NodeList**

- 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체

- childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.

## 요소 노드의 텍스트 조작

# nodeValue

- setter, getter 모두 존재하는 접근자 프로퍼티

- 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 차조하면 null을 반환한다.

## DOM 조작

# insertAdjacentHTML 메서드 -- quiz

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

## quiz

요소 노드 생성 : Document.prototype.createElement(tagName)

텍스트 노드 생성: Document.prototype.createTextNode(text)

마지막 노드로 추가: Node.prototype.appendChild

지정한 위치에 노드 삽입: Node.prototype.insertBefore(newNode, childNode)

노드 복사 : Node.prototype.cloneNode([deep: true | false])

# classList

- Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.

- add(...className)
  add 메서드는 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트로 추가한다.

```js
$box.classList.add("foo"); // => class="box red foo"

$box.classList.add("bar", "baz"); // => class= "box red foo bar baz"
```

-remove(...className)
remove 메서드는 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제한다. 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 없으면 에러 없이 무시된다.

-item(index)
item 메서드는 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다.

-contains(className)
contains 메서드는 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되지 있는지 확인한다.

- repalce(oldClassName, newClassName)
  replace 메서드는 class 어트리뷰트에서 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.

- toggle(className[.force])
  toggle 메서드는 class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
