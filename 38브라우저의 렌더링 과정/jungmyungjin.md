# 2023.04.04 자바스크립트 딥다이브 (38.브라우저의 렌더링 과정)

## 기억하고 싶은 내용

### 브라우저의 렌더링 과정

1. 브라우저는 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.
2. 브라우저의 렝더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성한다.
3. 브라우저의 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성한다
4. 렌더 트리를 기반으로 HTML 요소의 레잉아웃을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.



### 브라우저의 요청과 응답

**브라우저의 핵심 기능은 필요한 리소스(HTML, CSS, 자바스크립트, 이미지, 폰트 등)를 서버에 요청하고 서버로부터 응답받아 브라우저에 시각적으로 렌더링 하는 것이다.**

- 즉, 렌더링에 필요한 리소스는 모두 서버에 존재하므로 필요한 리소스를 서버에 요청하고 서버가 응답한 리소스를 파싱하여 렌더링 하는 것이다.



## 브라우저와의 요청/응답 시나리오

1. 브라우저 주소창에 https://website.com을 입력한다.
2. 루트 요청(/, scheme과 host 만으로 구성된  URI에 의한 요청)이 webstie.com 서버로 전송된다.
3. 루트 요청에는 명확히 리소스를 요청하는 내용이 없지만 일반적으로 서버는 루트 요청에 대해 암묵적으로 `index.html`을 응답하도록 기본 설정되어 있다.
4. 서버는 루트 요청에 따라 서버의 루트 폴더에 존재하는 정적파일 `index.html`을 클라이언트로 응답한다.
   - 반드시 브라우저의 주소창을 통해 서버에게 정작 파일만을 요청할 수 있는 것은 아니다. 자바스크립트를 통해 동적으로 서버에  `정적/동적` 데이터를 요청할 수도 있다.



### HTML 파싱과 DOM

- 브라우저의 요청에 의해 서버가 응답한 HTML 문서는 문자열로 이루어진 순수한 텍스트다.
- 순수한 텍스트인 HTML 문서를 브라우저에 시각적인 픽셀로 렌더링하려면 HTML문서를 브라우저가 이해할 수 있는 자료구조(객체)로 변환하여 메모리에 저장해야 한다.
- 브라우저의 렌더링 엔진은 응답받은 HTML문서를 파싱하여 브라우저가 이해 할 수 있는 자료구조인 **DOM**(Document Object Model)을 생성한다
- **즉, DOM은 HTML문서를 파싱한 결과물이다.**



### CSS 파싱과 CSSOM 생성

- 렌더링 엔진은 HTML을 처음부터 한 줄씩 순차적으로 파싱하여 DOM을 생성해 나간다.
- 이처럼 렌더링 엔진은 DOM을 생성해나가다 CSS를 로드하는 `link`태그나 `style`태그를 만나면 DOM 생성을 일시 중단한다.
- 그리고 `link` 태그의 `href` 어트리뷰트에 지정된 CSS파일을 서버에 요청하여 로드한 CSS파일이나 `style` 태그 내의 CSS를 HTML과 동일한 파싱과정을 거치며 해석하여 **CSSOM**(CSS Object Model)을 생성한다.



### 렌더 트리

- 레이아웃 계산과 페인팅을 다시 실행하는 리렌더링은 비용이 많이 드는, 즉 성능에 악영향을 주는 작업이다.
- 따라서 가급적 리렌더링이 빈번하게 발생하지 않도록 주의할 필요가 있다.



### 자바스크립트 파싱에 의한 HTML 파싱 중단

- 브라우저는 동기적(Synchronous)으로, 위에서 아래 방향을 순차적으로 HTML, CSS, 자바스크립트를 파싱하고 실행한다.

- 이때, `body`위 쪽의 `script` 태그의 위치에 생성이 지연된다면 블로킹이 발생할 수 있다.
- 이러한 문제를 회피하기 위해 `body`요소의 가장 아래에 자바스크립트를 위치시키는 것은 좋은 아이디어 이다.



### script 태그의 비동기적 생성 (async/defer)

```html
<script async src="extern1.js"></script>
<script defer src="extern2.js"></script>
```

**`async`**

- 자바스크립트의 파싱과 실행은 파일의 로드가 완료된 직후 진행되며, 이때 파싱이 중단된다
- 여러 개의 `script` 태그에 `async` 어트리뷰트를 지정하면 `script` 태그의 순서와는 상관없이 로드가 완료된 자바스크립트부터 먼저 실행되므로 순서가 보장되지 않는다.



**`defer`**

- 자바스크립트의 파싱과 실행은 HTML 파싱이 완료된 직후, 즉 DOM 생성이 완료된 직후 진행된다.
- <u>DOM 생성이 완료된 이후 실행되어야 할 자바스크립트에 유용하다.</u>



## 느낀점

- 책을 읽으면서 브라우저가 렌더링 하는 과정의 흐름에 대해 파악할 수 있었다.
- 브라우저가 요청하고 응답한다.. 라는 정도의 대략적인 내용은 알고 있었으나, 요청하는 리소스는 어떤 것들이 있으며, 응답하는 방법에 어떤 것들이 있는지 자세하게 알게되어 좋았다.
- script 태그의 비동기 방식은 처음 들어봐서 새로웠다.
- 알고있는 내용이지만 시기에 따라 반복하여 읽고싶은 내용인것 같다.
