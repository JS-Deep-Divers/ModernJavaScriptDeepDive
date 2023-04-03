# Quiz1 문제

1. 다음의 코드에서 에러가 난 이유와 어떠한 에러가 발생했는지 작성한 뒤, 에러가 발생한 코드를 올바르게 고쳐주세요.

```js
var person = {
    scienceBook : 'Ung-mo',
    korean-book : 'Lee';
}
```

<br>

2. 다음의 코드에서 에러가 난 이유와 어떠한 에러가 발생했는지 작성한 뒤 에러가 발생한 코드를 올바르게 고쳐주세요.

```js
var person = {
  name: 'Lee',
};

console.log(person.age);
console.log(person[name]);
```

<br>

3. console.log를 했을 때 나오는 결과를 작성해주세요.

```js
var str = 'gOOD MORNING';
var arr = ['h', 'E', 'L', 'L', 'O'];

str[0] = 'G';
arr[0] = 'H';

console.log(str);
console.log(arr);
```

<br>

4. console.log를 했을 때 나오는 결과를 작성해주세요.

```js
var school = {
    teacher : '홍길동';
    name : '엘리스';
}

var pencil_price = 1000;
var note_price = pencil_price;
var copy = school;

pencil_price = 700;

console.log(pencil_price);
console.log(note_price);

copy.teacher = '나철수';
school.name = '성수낙낙';

console.log(school.teacher);
console.log(copy.name);
```

_추가 문제(그림으로 그려서 풀어보면 머리에 잘 남는듯해서 각자 해보면 좋을 것 같습니다.)_

- 아래 코드의 변수 선언, 값 할당, 재할당의 과정을 메모리 공간으로 표현해보세요! (원시 값은 변경 불가능한 값이라는 개념을 잘 생각보시면 됩니다)

```js
var score;
score = 80;
score = 90;
```
