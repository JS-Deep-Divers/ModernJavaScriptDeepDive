### 새롭게 알게된 것!

[암묵적 타입 변환 또는 타입 강제 변환]

```
var str = x + '';
console.log(typeof str, str)  //string 10

```

[단축 평가]

```
true || anything  //  true
false || anything  // anything
true && anything  //  anything
false && anything  //  false

```

[옵셔널 체이닝 연산자]
`?.` 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용
