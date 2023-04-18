# 40장. 이벤트

## 40.1 이벤트 드리븐 프로그래밍

브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생  
&rarr; 클릭, 키보드 입력, 마우스 이동 등

**이벤트 핸들러**: 이벤트가 발생했을 때 호출될 함수  
**이벤트 핸들러 등록**: 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것  
**이벤트 드리븐 프로그래밍**: 프로그램 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식

## 40.2 이벤트 타입

이벤트 타입: 이벤트의 종류를 나타내는 문자열  
이벤트 타입에 대한 상세 목록 [🔗](https://developer.mozilla.org/en-US/docs/Web/Events)

### 40.2.1 마우스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                            |
| :---------- | :---------------------------------------------------------- |
| click       | 마우스 버튼을 클릭했을 때                                   |
| dblclick    | 마우스 버튼을 더블 클릭했을 때                              |
| mousedown   | 마우스 버튼을 눌렀을 때                                     |
| mouseup     | 누르고 있던 마우스 버튼을 놓았을 때                         |
| mousemove   | 마우스 커서를 움직였을 때                                   |
| mouseenter  | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링되지 않음) |
| mouseover   | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링됨)        |
| mouseleave  | 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링되지 않음) |
| mouseout    | 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링됨)        |

### 40.2.2 키보드 이벤트

| 이벤트 타입 | 이벤트 발생 시점                        |
| :---------- | :-------------------------------------- |
| keydown     | 모든 키를 눌렀을 때 발생                |
| keypress    | 문자 키를 눌렀을 때 연속적으로 발생     |
| keyup       | 누르고 있던 키를 놓았을 때 한 번만 발생 |

### 40.2.3 포커스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                |
| :---------- | :---------------------------------------------- |
| focus       | HTML 요소가 포커스를 받았을 때(버블링되지 않음) |
| blur        | HTML 요소가 포커스를 잃었을 때(버블링되지 않음) |
| focusin     | HTML 요소가 포커스를 받았을 때(버블링됨)        |
| focusout    | HTML 요소가 포커스를 잃었을 때(버블링됨)        |

focusin, focusout 이벤트 핸들러는 addEventListener 메서드 방식을 사용해 등록

### 40.2.4 폼 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                                                                                                         |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- |
| submit      | 1. form 요소 내의 input, select 입력 필드에서 엔터 키를 눌렀을 때<br>2. form 요소 내의 submit 버튼을 클릭했을 때<br>※ submit 이벤트는 form 요소에서 발생 |
| reset       | form 요소 내의 reset 버튼을 클릭했을 때(최근에는 사용 안 함)                                                                                             |

### 40.2.5 값 변경 이벤트

| 이벤트 타입      | 이벤트 발생 시점                                                                      |
| :--------------- | :------------------------------------------------------------------------------------ |
| input            | input, select, textarea 요소의 값이 입력되었을 때                                     |
| change           | input, select, textarea 요소의 값이 변경되었을 때                                     |
| readystatechange | HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티의 값이 변경될 때 |

### 40.2.6 DOM 뮤테이션 이벤트

| 이벤트 타입      | 이벤트 발생 시점                                            |
| :--------------- | :---------------------------------------------------------- |
| DOMContentLoaded | HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을 때 |

### 40.2.7 뷰 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                |
| :---------- | :-------------------------------------------------------------- |
| resize      | 브라우저 윈도우(window)의 크기를 리사이즈할 때 연속적으로 발생  |
| scroll      | 웹페이지(document) 또는 HTML 요소를 스크롤할 떄 연속적으로 발생 |

### 40.2.8 리소스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                          |
| :---------- | :------------------------------------------------------------------------ |
| load        | DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스의 로딩이 완료되었을 때 |
| unload      | 리소스가 언로드될 때                                                      |
| abort       | 리소스 로딩이 중단되었을 때                                               |
| error       | 리소스 로딩이 실패했을 때                                                 |

## 40.3 이벤트 핸들러 등록

이벤트 핸들러: 이벤트가 발생하면 브라우저에 의해 호출될 함수  
이벤트 핸들러 등록: 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임

이벤트 핸들러를 등록하는 방법 3가지

1. 이벤트 핸들러 어트리뷰트 방식
2. 이벤트 핸들러 프로퍼티 방식
3. addEventListener 메서드 방식

### 40.3.1 이벤트 핸들러 어트리뷰트 방식

HTML 요소의 어트리뷰트 중, 이벤트에 대응하는 이벤트 핸들러 어트리뷰트 존재  
이벤트 핸들러 어트리뷰트 이름은 이벤트의 종류를 나타내는 이벤트 타입으로 이루어짐  
이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문을 할당하면 이벤트 핸들러가 등록

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="sayHi('Lee')">Click me!</button>
    <script>
      function sayHi(name) {
        console.log(`Hi! ${name}.`);
      }
    </script>
  </body>
</html>
```

! 주의  
이벤트 핸들러의 어트리뷰트 값으로 함수 참조가 아닌 함수 호출문 등의 문을 할당
위 예제에서 이벤트 핸들러 어트리뷰트 값은 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미

### 40.3.2 이벤트 핸들러 프로퍼티 방식

window 객체와 Document, HTMLElement 타입의 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티 보유  
이벤트 핸들러 프로퍼티의 키는 이벤트의 종류를 나타내는 이벤트 타입으로 이루어짐  
이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
      $button.onclick = function () {
        console.log("button click");
      };
    </script>
  </body>
</html>
```

이벤트 핸들러를 등록하기 위해 이벤트 타깃, 이벤트 타입, 이벤트 핸들러 지정 필요

- 이벤트 타깃: 이벤트를 발생시킬 객체, `$button`
- 이벤트 타입: 이벤트의 종류를 나타내는 문자열, `onclick`
- 이벤트 핸들러: 이벤트가 발생하면 브라우저에 의해 호출될 함수, `function`

이벤트 핸들러 프로퍼티 방식의 장단점

- 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제 해결
- 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩 가능

### 40.3.3 addEventListener 메서드 방식

DOM Level 2에서 도입  
EventTarget.prototype.addEventListener 메서드를 사용하여 이벤트 핸들러 등록  
(이벤트 핸들러 어트리뷰트 방식, 이벤트 핸들러 프로퍼티 방식은 DOM Level 0부터 제공되던 방식)

`EventTarget.addEventListener('eventType', functionName [, useCapture]);`

- 첫 번째 매개변수: 이벤트 종류를 나타내는 문자열인 이벤트 타입(on 접두사를 붙이지 않음)
- 두 번째 매개변수: 이벤트 핸들러
- 이벤트를 캐치할 이벤트 전파 단계를 지정  
  &rarr; 생략 or false: 버블링 단계에서 이벤트 캐치  
  &rarr; true: 캡처링 단계에서 이벤트 캐치

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // addEventListener 메서드 방식
      $button.addEventListener("click", function () {
        console.log("button click");
      });
    </script>
  </body>
</html>
```

동일한 HTML 요소에서 발생한 동일한 이벤트에 대해 addEventListener 메서드는 하나 이상의 이벤트 핸들러를 등록 가능(등록된 순서로 호출)

## 40.4 이벤트 핸들러 제거

addEventListener 메서드로 등록한 이벤트 핸들러를 제거하려면 EventTarget.prototype.removeEventListener 메서드를 사용

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 이벤트 핸들러 등록
      $button.addEventListener("click", handleClick);

      // 이벤트 핸들러 제거
      // addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에
      // 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않음
      $button.removeEventListener("click", handleClick, true); // 실패
      $button.removeEventListener("click", handleClick); // 성공
    </script>
  </body>
</html>
```

이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener 메서드로 제거 불가, 이벤트 핸들러 프로퍼티에 null을 할당하여 제거

## 40.5 이벤트 객체

이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성  
생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달

```html
<!DOCTYPE html>
<html>
  <body>
    <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
    <em class="message"></em>
    <script>
      const $msg = document.querySelector(".message");

      const handleClick = () => console.log("button click");

      // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달
      function showCoords(e) {
        $msg.textContent = `clientX: ${e.clientX}, client?Y: ${e.clientY}`;
      }

      document.onclick = showCoords;
    </script>
  </body>
</html>
```

클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 e에 암묵적으로 할당  
이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할 때 이벤트 객체를 전달받을 매개변수를 명시적으로 선언 필요(예제의 매개변수 e, 다른 이름 사용 가능)

### 40.5.1 이벤트 객체의 상속 구조

이벤트가 발생하면 이벤트 타입에 따라 다양한 타입의 이벤트 객체가 생성

이벤트 객체의 상속 구조

```text
                 |-[AnimationEvent] |-[FocusEvent   ]
[Object]-[Event]-|-[UIEvent       ]-|-[MouseEvent   ]-|-[DragEvent]
                 |-[ClipboardEvent] |-[KeyboardEvent] |-[WheelEvent]
                 |-[CustomEvent   ] |-[InputEvent   ]
                 |-...              |-[TouchEvent   ]
```

Event. UIEvent, MouseEvent 등 모두 생성자 함수, 따라서 생성자 함수를 호출하여 이벤트 객체 생성 가능  
이벤트 객체가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성  
생성된 이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원

이벤트 객체 중 일부는 사용자의 행위에 의해 생성, 일부는 자바스크립트 코드에 의해 인위적으로 생성

### 40.5.2 이벤트 객체의 공통 프로퍼티

Event 인터페이스, 즉 Event.prototype에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, customEvent, MouseEvent 등 모든 파생 이벤트 객체에 상속  
Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티

| 공통 프로퍼티    | 설명                                                                                                                                                                                                                                                | 타입          |
| :--------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------ |
| type             | 이벤트 타입                                                                                                                                                                                                                                         | string        |
| target           | 이벤트를 발생시킨 DOM 요소                                                                                                                                                                                                                          | DOM 요소 노드 |
| currentTarget    | 이벤트 핸들러가 바인딩된 DOM 요소                                                                                                                                                                                                                   | DOM 요소 노드 |
| eventPhase       | 이벤트 전파 단계                                                                                                                                                                                                                                    | number        |
| bubbles          | 이벤트를 버블링으로 전파하는지 여부. 다음 이벤트는 bubbles: false로 버블링하지 않음<br>- 포커스 이벤트 focus/blur<br>- 리소스 이벤트 load/unload/abort/error<br>- 마우스 이벤트 mouseenter/mouseleave                                               | boolean       |
| cancelable       | preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부. 다음 이벤트는 cancelable: false로 취소 불가<br>- 포커스 이벤트 focus/blur<br>- 리소스 이벤트 load/unload/abort/error<br>- 마우스 이벤트 dblclick/mouseenter/mouseleave | boolean       |
| defaultPrevented | preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부                                                                                                                                                                                           | boolean       |
| isTrusted        | 사용자의 행위에 의해 발생한 이벤트인지 여부                                                                                                                                                                                                         | boolean       |
| timeStamp        | 이벤트가 발생한 시각(1970/01/01/00:00:0부터 경과한 밀리초)                                                                                                                                                                                          | number        |

### 40.5.3 마우스 정보 취득

click, dblclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 이벤트 객체는 다음과 같은 고유의 프로퍼티를 보유

- 마우스 포인터의 좌표 정보를 나타내는 프로퍼티  
  screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY.
- 버튼 정보를 나타내는 프로퍼티
  altKey, ctrlKey, shiftKey, button

### 40.5.4 키보드 정보 취득

keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로퍼티를 보유

## 40.6 이벤트 전파

이벤트 전파: DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파

- 캡처링 단계: 이벤트가 상위 요소에서 하위 요소 방향으로 전파
- 타깃 단계: 이벤트가 이벤트 타깃에 도달
- 버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파

이벤트는 이벤트를 발생시킨 이벤트 타깃을 물론 상위 DOM 요소에서도 캐치 가능  
DOM 트리를 통해 전파되는 이벤트는 이벤트 패스에 위치한 모든 DOM 요소에서 캐치할 수 있음

대부분의 이벤트는 캡처링과 버블링을 통해 전파  
다음 이벤트는 버블링을 통해 전파되지 않음(event.bubbles의 값이 모두 false)

- 포커스 이벤트: focus/blur
- 리소스 이벤트: load/unload/abort/error
- 마우스 이벤트 mouseenter/mouseleave

이벤트 타깃의 상위 요소에서 이벤트를 캐치하려면 캡처링 단계의 이벤트 캐치 필요  
반드시 이벤트를 상위 요소에서 캐치해야할 경우 대체할 수 이벤트 존재

- focus/blur &rarr; focusin/focusout
- moseenter/mouseleave &rarr; mouseover/mouseout

## 40.7 이벤트 위임

이벤트 위임: 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법

이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록  
&rarr; 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요 없음  
&rarr; 동적으로 하위 DOM 요소 추가하더라도 추가된 DOM 요소에 이벤트 핸들러 등록할 필요 없음

## 40.8 DOM 요소의 기본 동작 조작

### 40.8.1 DOM 요소의 기본 동작 중단

DOM 요소는 저마다의 기본 동작 존재  
이벤트 객체의 preventDefault 메서드는 이러한 DOM 요소의 기본 동작을 중단시킴

### 40.8.2 이벤트 전파 방지

이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지시킴
하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트의 전파를 중단시킴

## 40.9 이벤트 핸들러 내부의 this

### 40.9.1 이벤트 핸들러 어트리뷰트 방식

다음 예제의 handleClick 함수 내부의 this는 전역 객체 window를 가리킴

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="handleClick()">Click me</button>
    <script>
      function handleClick() {
        console.log(this); // window
      }
    </script>
  </body>
</html>
```

이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 사실 암묵적으로 생성되는 이벤트 핸들러의 문  
따라서 handleClick 함수는 이벤트 핸들러에 의해 일반 함수로 호출  
일반 함수로서 호출되는 함수 내부의 this는 전역 객체를 가리킴  
따라서 handleClick 함수 내부의 this는 전역 객체 window

### 40.9.2 이벤트 핸들러 어트리뷰트 방식과 addEventListener 메서드 방식

이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식 모두 이벤트 핸들러 내부의 this는 이벤트 바인딩한 DOM 요소를 가리킴  
이벤트 핸들러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 동일

화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킴(화살표 함수는 함수 자체의 this 바인딩을 갖지 않음)

클래스에서 이벤트 핸들러를 바인딩하는 경우 this에 주의

## 40.10 이벤트 핸들러에 인수 전달

함수에 인수를 전달하려면 함수를 호출할 때 전달

- 이벤트 핸들러 어트리뷰트 방식  
   &rarr; 함수 호출문을 사용할 수 있기 때문에 인수 전달 가능
- 이벤트 핸들러 프로퍼티 방식, addEventListener 메서드 방식  
   &rarr; 이벤트 핸들러를 브라우저가 호출 불가, 따라서 함수 호출문이 아닌 함수 자체 등록 필요하므로 인수 전달 불가  
   (단, 이벤트 핸들러 내부에서 함수를 호출하면서 또는 이벤트 핸들러를 반환하는 함수를 호출하면서 인수 전달 가능)

## 40.11 커스텀 이벤트

### 40.11.1 커스텀 이벤트 생성

이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 발생한 이벤트의 종류에 따라 이벤트 타입이 결정  
Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입 지정 가능

커스텀 이벤트: 개발자의 의도로 생성된 이벤트

이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달  
이때 이벤트 타입을 나타내는 문자열은 기존 이벤트 타입 또는 임의의 문자열 사용 가능

```js
// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체르 생성
const keyboardEvent = new KeyboardEvent("keyup");
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo");
console.log(customEvent.type); // foo
```

생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수 없음  
즉 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티의 값이 false로 기본 설정

이벤트 객체 고유의 프로퍼티 값을 지정하려면 이벤트 생성자 함수의 두 번쨰 인수로 프로퍼티 전달

```js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const mouseEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
  clientX: 50,
  clientY: 100,
});

console.log(mouseEvent.clientX); // 50
console.log(mouseEvent.clientY); // 100

// KeyboardEvent 생성자 함수로 Keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent("keyup", { key: "Enter" });

console.log(keyboardEvent.key); // Enter
```

이벤트 생성자 함수로 생성한 커스텀 이벤트의 isTrusted 프로퍼티 값  
&rarr; false  
사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTrusted 프로퍼티 값  
&rarr; true

### 40.11.2 커스텀 이벤트 디스패치

생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있음  
dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생

일반적으로 이벤트 핸들러는 비동기 처리 방식으로 동작  
dispatchEvent 메서드는 이벤트 핸들러를 동기 처리 방식으로 호출  
따라서 dispatchEvent 메서드로 이벤트를 디스패치하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러 등록 필요
