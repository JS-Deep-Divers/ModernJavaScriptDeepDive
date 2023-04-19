# 47장. 에러 처리

## 47.1 에러 처리의 필요성

발생한 에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료  
try .. catch 문을 사용해 에러에 적절하게 대응하면 강제 종료되지 않고 계속 코드 실행 가능

```js
console.log("[Start]");

try {
  foo();
} catch (error) {
  console.error("[에러 발생]", error);
  // [에러 발생] ReferenceError: foo is not defined
}

// 발생한 에러에 적절한 대응을 하면 프로그램이 강제 종료되지 않음
console.log("[End]");
```

추가로 직접적으로 에러를 발생하지 않는 예외적인 상황 발생 가능성 존재  
이 또한 적절하게 대응하지 않으면 에러로 이어질 가능성 &uarr;

에러나 예외적인 상황에 대응하지 않으면 프로그램은 강제 종료  
에러, 예외적인 상황은 다양하여 조치 없이 프로그램이 강제 종료되면 원인 파악하여 대응하기 어려움  
작성한 코드에서는 언제나 에러, 예외적인 상황이 발생할 수 있다는 것을 전제하고 이에 대응하는 코드를 작성하는 것이 중요

## 47.2 try .. catch ... finally 문

기본적으로 에러 처리를 구현하는 방법은 크게 두 가지

- 예외적인 상황이 발생하면 반환하는 값을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
- 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법

try ... catch ... finally 문은 두 번째 방법에 해당  
일반적으로 이 방법을 '에러 처리'라고 함

try ... catch ... finally 문은 3개의 코드 블록으로 구성  
finally 문은 생략가능

```js
console.log("[Start]");

try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
  foo();
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달
  console.error(err); // ReferenceError: foo is not defined
} finally {
  // 에러 발생과 상관 없이 반드시 한번 실행
  console.log("finally");
}

// try...catch...finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않음
console.log("[End]");
```

## 47.3 Error 객체

Error 생성자 함수는 에러 객체를 생성  
Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달 가능

```js
const error = new Error("invalid");
```

자바스크립트는 Error 생성자 함수를 포함 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공

| 생성자 함수    | 인스턴스                                                                       |
| :------------- | :----------------------------------------------------------------------------- |
| Error          | 일반적 에러 객체                                                               |
| SyntaxError    | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체                |
| ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체                         |
| TypeError      | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체         |
| RangeError     | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체                            |
| URIError       | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
| EvalError      | eval 함수에서 발생하는 에러 객체                                               |

## 47.4 throw 문

Error 생성자 함수로 에러 객체를 생성한다고 에러 발생하지 않음  
에러 객체 생성과 에러 발생의 의미가 다름

에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 함

```js
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작
  throw new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

## 47.5 에러의 전파

에러는 호출자 방향으로 전파됨  
즉, 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파

```js
const foo = () => {
  throw Error("foo에서 발생한 에러"); // ④
};

const bar = () => {
  foo(); // ③
};

const baz = () => {
  bar(); // ②
};

try {
  baz(); // ①
} catch (err) {
  console.error(err);
}
```

① 에서 baz 함수를 호출하면 ②에서 bar 함수가 호출 &rarr; ③에서 foo 함수가 호출 &rarr; foo 함수는 ④에서 에러를 throw  
이때 foo 함수가 throw한 에러는 다음과 같이 호출자에게 전파되어 전역에서 캐치

```
|[foo 실행 컨텍스트]*️⃣ |❗️에러 발생
|[bar 실행 컨텍스트]⬇️ |
|[baz 실행 컨텍스트]⬇️ | 에러 전파(호출자 방향)
|[전역 실행 컨텍스트]⬇️ |
+------------------+
```

throw된 에러를 캐치하지 않으면 호출자 방향으로 전파  
이때 throw된 에러를 캐치하여 적절히 대응하면 프로그램을 강제 종료시키지 않고 코드의 실행 흐름 복구 가능

! 주의  
비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것

# 48장. 모듈

## 48.1 모듈의 일반적 의미

모듈: 애플리케이션을 구성하는 개별적 요소로서 재사용이 가능한 코드 조각을 말함

일반적으로 모듈은 기능을 기준으로 파일 단위로 분리  
모듈이 성립하려면 모듈은 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 함  
자신만의 파일 스코프를 갖는 모듈의 자산(변수, 함수, 객체 등)은 기본적으로 비공개 상태  
다른 모듈에서 접근 불가  
이를 다른 모듈에서 재사용하기 위해 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개 가능 &rarr; **export**
모듈 사용자는 모듈이 공개(export)한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용 가능 &rarr; **import**

모듈의 장점

- 코드의 단위를 명확히 분리하여 애플리케이션 구성 가능
- 재사용성이 좋아서 개발 효율성과 유지보수성 &uarr;

## 48.2 자바스크립트와 모듈

자바스크립트는 모듈 시스템을 지원하지 않음  
따라서 import, export 지원하지 않았음

자바스크립트를 범용적으로 사용하기 위해 모듈 시스템은 해결해야하는 핵심 과제로 부상  
이 상황에서 모듈 시스템 문제를 해결하기 위해 CommonJS와 AMD(Asynchronous Module Definition) 제안  
자바스크립트 런타임 환경인 Node.js는 모듈 시스템의 사실상 표준인 CommonJS 채택  
Node.js는 ECMAScript 표준 사양은 아니지만 모듈 시스템 지원  
따라서 Node.js 환경에서는 파일별로 독립적인 파일 스코프(모듈 스코프)를 가짐

## 48.3 ES6 모듈(ESM)

ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능 추가

**ES6 모듈(ESM)의 사용법**  
script 태그에 type="module" 어트리뷰트를 추가, 로드된 자바스크립트 파일은 모듈로서 동작  
ESM임을 명확히하기 위해 파일 확장자는 mjs 사용할 것을 권장  
기본적으로 strict mode 적용

```html
<script type="module" src="app.mjs"></script>
```

### 48.3.1 모듈 스코프

- ESM이 아닌 일반적인 자바스크립트 파일  
  &rarr; script 태그로 분리해서 로드해도 독자적인 모듈 스코프를 갖지 않음
- ESM 파일  
  &rarr; 파일 자체의 독자적인 모듈 스코프를 제공  
  &rarr; 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아님

모듈 내에서 선언한 식별자는 모듈 외부에서 참조 불가(모듈 스코프가 다름)

### 48.3.2 export 키워드

모듈은 독자적인 모듈 스코프를 가지기 때문에 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부에서만 참조 가능  
다른 모듈들이 재사용할 수 있게하려면 export 키워드 사용

export 키워드는 선언문 앞에 사용  
변수, 함수, 클래스 등 모든 식별자 export 가능

```js
// lib.mjs
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}

// 클래스의 공개
export class Person {
  constructor(name) {
    this.name = name;
  }
}
```

export 대상을 하나의 객체로 구성하여 한 번에 export

```js
// lib.mjs
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

### 48.3.3 import 키워드

다른 모듈에서 공개(export)한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드 사용  
다른 모듈이 export한 식별자 이름으로 import 필요  
ESM의 경우 파일 확장자를 생략 불가

```js
// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import
// ESM의 경우 파일 확장자를 생략 불가
import { pi, square, Person } from "./lib.mjs";

console.log(pi); // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person("Lee")); // Person { name: 'Lee' }
```

위 에제의 app.mjs는 애플리케이션의 진입점이므로 반드시 script 태그로 로드  
lib.mjs는 app.mjs의 import 문에 의해 로드되는 의존성, lib.mjs는 따로 로드하지 않음

하나의 이름으로 한 번에 import 가능

```js
// app.mjs
// lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import
import * as lib from "./lib.mjs";

console.log(lib.pi); // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person("Lee")); // Person { name: 'Lee' }
```

식별자 이름을 변경하여 import 가능

```js
// app.mjs
// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import
import { pi as PI, square as sq, Person as P } from "./lib.mjs";

console.log(PI); // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P("Kim")); // Person { name: 'Kim' }
```

**default 키워드**  
모듈에서 하나의 값만 export 가능  
기본적으로 이름 없이 하나의 값 export  
해당 키워드를 사용하면 var, let, const 키워드는 사용 불가  
default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import

```js
// lib.mjs
export default (x) => x * x;
```

```js
// app.mjs
import square from "./lib.mjs";

console.log(square(3)); // 9
```
