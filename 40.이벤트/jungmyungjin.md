# 2023.04.10  자바스크립트 딥다이브 (40.이벤트)

## 기억하고 싶은 내용

**이벤트 핸들러**

- <u>이벤트가 발생했을 때 호출될 함수</u>를 **이벤트 핸들러**라고 한다



**이벤트 드리븐 프로그래밍**

- 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 이벤트 드리븐 프로그래밍이라고 한다.



### 이벤트 핸들러

이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록이라 한다.

**이벤트 핸들러 등록 방법**

- **이벤트 핸들러 어트리뷰트 방식**

  - HTML 요소의 어트리뷰트에는 이벤트 핸들러 어트리뷰트가 있다.

  - 이벤트 핸들러 어트리뷰트의 이름은`onClick`과 같은 on 접두사와 이벤트 종류를 나타내는 이벤트 타입으로 이루어져 있다.

  - 오래된 코드에서 간혹 볼 수 있는 방식이다.

    ```html
    <body>
      <!-- 이때 이벤트 어트리뷰트 값은 사실 암묵적으로 이벤트 핸들러의 함수 몸체를 의미한다. -->
      <button onclick="sayHi('Jung')">
        CLick me!
      </button>
      <script>
        function sayHi(name){
          console.log(`Hi ${name}.`);
        }
      </script>
    </body>
    ```

    

- **이벤트 핸들러 프로퍼티 방식**

  - DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 갖고 있다.

  - 이벤트 핸들러 어트리뷰트의 이름은 이벤트 핸들러 어트리뷰트와 마찬가지로 `onClick`과 같은 on 접두사와 이벤트 종류를 나타내는 이벤트 타입으로 이루어져 있다.

  - 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록된다.

    ```html
    <body>
      <button>Click me!</button>
      <script>
        const $button = docuemnt.querySelector('button');
        
        // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
        $button.onclick = function (){
          console.log('button click');
        }
      </script>
    </body>
    ```

    

- **addEventListner 메서드 방식**

  - 앞의 방식보다 뒤에서 도입된 `EventTarget.prototype.addEventListner`메서드를 사용하여 이벤트 핸들러를 등록할 수 있다.
  - 마지막 매개변수에는 이벤트를 캐치할 이벤트 전파 단계(캡쳐링 or 버블링)을 지정한다.
  - `addEventListner` 메서드는 하나 이상의 이벤트 핸들러를 등록할 수 있다.
  - 참조가 동일한 이벤트 핸들러를 중복등록하면 하나의 핸들러만 등록된다.

```html
<body>
  <button>Click me!</button>
  <script>
    const $button = docuemnt.querySelector('button');
    
    // addEventLustner 방식
    $button.addEventListner('click', function(){
      console.log('button click 1');
    })
    $button.addEventListner('click', function(){
      console.log('button click 2');
    })
  </script>
</body>
```



### 이벤트 전파 event propagation

- DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파된다. 이를 이벤트 전파(event propagation)라고 한다.

- 이벤트가 생성되면 이벤트 객체는 이벤트를 발생시킨 DOM요소인 이번트 타깃 event target을 중심으로 DOM 트리를통해 전파된다.



### 이벤트 전파 단계

1. 캡처링 단계 (capturing phase)
   - 이벤트가 상위 요소에서 하위 요소 방향으로 전파
2. 타깃 단계(target phase)
   - 이벤트가 이벤트 타깃에 도달
3. 버블링 단계(bubbling phase)
   - 이벤트가 하위 요소에서 상위 요소로 전파



DOM 요소 관련 메소드

- **`preventDefault` 메서드**
  - DOM 요소의 기본 동작을 중지시킨다
- **`stopProgagation`** 메서드
  - 이벤트 전파를 중지시킨다.





## 느낀점

- 바닐라JS는 DOM과 Event가 주 내용이어서 그럴까... 이번 Event 파트 내용도 길어서 생각보다 오래 읽게 되었다. ~~힘들었돱~~
- 이번에 인상 깊었던 내용은 이벤트 핸들러 등록 방식이다.
  - 이전에 이벤트 관련 코드를 작성할 때 같은 결과물을 내는데 여러가지 방식이 있어서 왜 그런지 이해도 안되었기 때문이다
  - 핸들 방식이 나온 순서까지 보고 그 활용 방법 및 장단점들을 살펴보니, 충분히 이해가 되었다 ~~웬만하면 addEventListner를 써야지 라는...?~~
- 앨리스에서 공부할때의 초반 내용인데, 책으로 다시 이 내용을 알게되니 새삼 색다른 기분이다.
- 그래도 나중에 바닐라JS에 익숙해지고 읽으면 좀 더 안보였던것, 몰랐던 것 을 느낄것 같다.



