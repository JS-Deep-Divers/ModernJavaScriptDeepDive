# Quiz1 정답

1. korean-book이 식별자 네이밍 규칙을 따르지 않아 SyntaxError가 발생한다. 따라서 'korean-book'으로 키를 설정해주어야 한다.

2. name은 식별자가 아니기 때문에 데이터에 접근 시 따옴표를 작성해주어야 합니다. 현 코드에서는 ReferenceError가 발생하며 person['name']으로 수정해야 합니다.

3. string은 유사배열객체로 배열과 비슷하게 인덱스 접근이나 length 프로퍼티를 갖지만 원시타입이기 때문에 원시값을 변경하는 것은 불가능합니다. 따라서 결과는 아래와 같습니다.

```js
console.log(str); // 'good morning'
console.log(arr); // ['H','E','L','L','O']
```

4.

```js
console.log(pencil_price); // 700
console.log(note_price); // 1000
console.log(school.teacher); //나철수
console.log(copy.name); //성수낙낙
```

추가문제 답

- 페이지 139쪽의 [그림 11-]1 원시 값은 변경 불가능한 값이다를 참고해주세요.
