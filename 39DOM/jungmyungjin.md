# 2023.04.05 자바스크립트 딥다이브 (39.DOM)

## 기억하고 싶은 내용

### DOM에서 중요한 노드 타입 4가지

**문서노드 Document node**

- 문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킨다.
- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체이다.
- document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점(**entry point**)역할을 담당한다.
- 즉 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.



**요소노드 Element node**

- HTML 요소를 가리키는 객체다.
- 요소 노드는 문서의 구조를 표현한다.



**어트리뷰트 Attribute node**

- 어트리뷰트 노드는 HTML 요소를 가리키는 객체이다.
- 어트리뷰트 노드는 어트리뷰트가 지정된 `HTML요소`의 `요소 노드`와 연결되어 있다.



**텍스트 노드 Text node**

- 텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체이다.
- 텍스트 노든느 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드(`leaf node`)이다.



### DOM 메서드

**getElementById**

- `getElementById` 메서드는 `Document.prototype`의 프로퍼티다. 따라서 반드시 문서 노드인 `document`를 통해 호출해야 한다.



**getElementsByTagname, getElementsByTagName,**

- 여러 개의 요소 노드 객체를 갖는 **DOM 컬렉션 객체인 `HTMLCollection` 객체를 반환**한다.
- DOM 컬렉션 객체인 `HTMLCollection` 객체는 유사 배열 객체이면서 이터러블이다.



**querySelector**

- 인수로 전덜한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.



**querySelectorAll**

- 인수로 전달한 CSS선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다.
- `querySelectorAll` 메서드는 여러 개의 요소 노드 객체를 갖는 **DOM 컬렉션 객체인 `NodeList` 객체를 반환**한다.
- `NodeList`객체는 유사 배열 객체이면서 이터러블이다.



### HTTPCollection 과 NodeList

DOM 컬렉션 객체인 `HTTPCollection`과 `NodeList`은 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체이다.

- `HTTPCollection`과 `NodeList`은 모두 유사 배열 객체이면서 이터러블이다.
- <u>노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 **`HTMLCollection`이나 `NodeList` 객체를 배열로 변환하여 사용하는 것을 권장**<u>한다.



**HTTPCollection**

- 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체다.
- 따라서 `HTTPCollection`객체를 살아있는(live) 객체라고 부르기도 한다.



**NodeList**

- NodeList 객체는 실시간으로 노드 객체 상태 변경을 하지 않는(non-live) 객체다.
- `HTMLCollection` 객체의 부작용을 해결하기 위해 NodeList를 반환하는 `querySellectorAll` 메서드를 사용하는 방법도 있다.
- 하지만 `childNodes` 프로퍼티가 반환하는 `NodeList` 객체는 `HTTPCollection` 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.



### DOM조작

**innerHTML**

- `innerHTML` 프로퍼티에 HTML 마크업 문자열을 할당하면 **유지되어도 좋은 기존의 자식 노드까지 모두 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 DOM에 반영한다.**
  - 이는 효율적이지 않다.

**insertAdjacentHTML**

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

**creatDocumentFragment**

- 요소 생성 시 DOM에 반영할 때 여러번 추가하게 되면 리플로우와 리페인팅을 여러번 실행하게 되어 높은 처리비용이 들기 때문에 가급적 횟수를 줄이는 편이 좋다.
- 때문에 컨테이너 요소에 원하는 만큼 추가 후 마지막에 DOM으로 추가하는 방법이 바람직하다
- `createElement` 요소를 컨테이너 요소로 생성할 경우 불필요한 div가 생성되므로 바람직하지 않다.
- **`DocumentFragment` 노드에 DOM을 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가된다.**
- 여러개의 요소 노드를 DOM에 추가하는 경우 `DocumentFragment` 노드를 사용하는 것이 더 효율적이다.



### 어트리뷰트 조작

**HTML 요소의 attributes 프로퍼티의 조작**

```javascript
const input = document.getElementById('user');
const inputValue = input.getAttribute('value');
```



**DOM 프로퍼티 attribute 조작**

```javascript
const input = document.getElementById('user');
const inputValue = input.value
```



**HTML 요소의 어트리뷰트**

- HTML 어트리뷰트의 역할은 HTML요소의 초기 상태를 지정하는 것이다.
- HTML 어트리뷰트 값은 **초기 상태를 의미하며 변하지 않는다.**



**DOM 어트리뷰트**

- 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리한다.
- DOM 프로퍼티는 **사용자 입력에 의한 상태 변화에 반응하여 언제나 최신상태를 유지한다.**



### data 어트리뷰트와 dataset 프로퍼티

- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
- data 어트리뷰트는 `data-user-id`, `data-role`과 같이 **data-**접두사 다음에 임의의 이름을 붙여 사용한다.
- data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML요소에 data 어트리뷰트가 추가된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul class="users">
      <li id="1" data-user-id="7621" data-role="admin">Lee</li>
      <li id="2" data-user-id="9524" data-role="subscriber">Kim</li>
    </ul>
  </body>
  <script>
    const user = user.find(user=> user.dataset.userId === '7621');
    console.log(user.dataset.role); // 'admin'
  </script>
</html>
```









## 느낀점

- 바닐라JS로 프론트를 공부할 때 써보았던 DOM이었지만, 원리나 개념 부분을 깊게 공부하지 않고 사용에만 초점을 맞추었기에 이번 장에서는 헷깔리거나 애매하게 알았던 것을 자갈속 모래를 채우듯 메꿔가는 시간이었던것 같다.
- 특히 읽으면서 알게되어 좋았던 부분은 `HTTPCollection`과 `NodeList` 이다.
  - 이전에 바닐라JS 공부 할 때 이 둘의 불러오는 방식의 차이 때문에 버그를 많이 만났기 때문이다.

- 챗GPT한테 물어보고 알아봤지만 그냥 차이 때문에 버그가 난다는 것만 알았고, 당시엔 해결에만 급급했기 때문에 더 자세히 알아보지는 않았던것 같다.
- ~~DOM에 대한 양 많아서 읽으면서 살짝지치긴했다ㅎㅎㅎㅎㅎ~~
- 읽는 양에 있어서는 힘들었지만, 많은 도움이 된 파트였다.

