- strict mode의 적용
strict mode을 적용하려면 전역의 선두 또는 함수 몸체 선두에 'use strict'; 를 추가
전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.

1. 전역에 strict mode를 적용X
strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은
오류를 발생시킬 수도 있고
외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode 인 경우도
있기 때문에 전역 strict mode를 적용하는 것은 바람직하지 않다.

이러한 경우 실행함수로 스크립트 전체를 감사서 스코프를 구분하고
즉시 실행 함수 선두에 strict mode를 적용한다.

2. 함수 단위로 strict mode를 적용X
일부 함수에만 strict mode를 적용하는 것도 바람직하지 않다.
모든 함수에 strict mode를 적용하는 것도 번거롭다.

strict mode가 적용된 함수가 참조할 함수 외부 컨텍스트에
strict mode를 적용하지 않는다면 문제가 발생할 수 있다.


- 화살표 함수 this

상위 스코프의 this

```javascript
const value = 1;

const obj = {
  value: 100,
  arrowFn() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100);
  }
};

//debugger;

obj.arrowFn();
```


- this binding

bind, apply, call

```javascript
const displayThis = function (key, value) {
  console.dir(this);
  console.log("key",key);
  console.log("value", value);
};

//debugger;

displayThis();

const obj = { displayThis };
obj.displayThis();

const bindingObject = { name: 'binding' };
displayThis.call(bindingObject, 'language', 'javascript');
displayThis.apply(bindingObject, ['language', 'typescript']);
displayThis.bind(bindingObject)();
```

- 생성자 함수 this

1. new 키워드 없을 경우 일반 함수처럼 동작하고 리턴이 없을 경우 undefined반환
2. new 키워드 있을 경우 this를 해당 object로 binding해주고 리턴을 명시하지 않아도 암묵적으로 (해당 object) this를 반환


