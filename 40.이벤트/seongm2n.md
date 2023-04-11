### 40

[40.2] 이벤트 타입

(3) 포커스 이벤트

- focusin, focusout 이벤트 핸들러는 addEventListener메서드 방식을 사용해 등록

[40.3] 이벤트 핸들러 등록

- 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러 호출을 위임하는 것
- 함수 호출을 브라우저에게 위임하는 것
- 이벤트 핸들러를 등록할 때 콜백 함수와 마찬가지로 함수 참조를 등록해야 브라우저가 이벤트 핸들러 호출

(1) 이벤트 핸들러 어트리뷰트 방식

- onclick과 같이 on 접두사와 이벤트 종류를 나타내는 이벤트 타입
- **이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미**

❗️ 이 방식은 오래된 코드에서 간혹 이 방식을 사용한 것이 있기 때문에 알아둘 필요는 있지만 더는 사용하지 않는 것이 좋음

(2) 이벤트 핸들러 프로퍼티 방식

- 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제 해결
- 단점 : 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있음

(3) addEventListener 메서드 방식

- EventTarget.prototype.addEventListener 메서드 사용해서 이벤트 핸들러 등록
    - EventTarget.addEventListener(’eventType’, functionName[,useCapture]);
- on 접두사 붙이지 않음

[40.6] 이벤트 전파😵‍💫하아 ㅠ 뭔가 어렵고 안읽히는 ㅠ생각할수록 이해가 안가아아아

- 생성된 이벤트 객체는 이벤트를 발생시킨 DOM요소인 이벤트 타깃을 중심으로 DOM트리를 통해 전파

    1️⃣ Capturing phase (캡처링 단계) : 이벤트가 상위 요소에서 하위요소 방향으로 전파

    2️⃣ Target phase(타깃 단계) : 이벤트가 이벤트 타깃에 도달

    3️⃣ 버블링 단계(bubling phase): 이벤트가 하위 요소에서 상위 요소 방향으로 전파

- 버블링 되지 않는 이벤트 타깃
    - 포커스 이벤트: focus/blur
    - 리소스 이벤트: load/unload/abort/error
    - 마우스 이벤트: mouseenter/mouseleave
- 버블링 되는 이벤트
    - focusin/focusout
    - mouseover/mouseout

[40.8] DOM 요소의 기본 동작 조작

(1) DOM 요소의 기본 동작 중단

- preventDefault 메서드는 기본동작을 중단시킴

(2) 이벤트 전파 방지

- stopPropagation 메서드로 이벤트 전파 중지
