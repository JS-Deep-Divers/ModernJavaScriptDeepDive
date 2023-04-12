# 28, 29, 30, 31장 Quiz

## 문제1. 아래 코드의 결과를 작성해주세요.

```js
Number.isFinite(infinity);
Number.isFinite(Number.MAX_SAFE_INTEGER);
```

<br>

## 문제2. 아래 코드의 결과를 작성해주세요.

```js
new Date('2023/04/03').getMonth();
```

<br>

## 문제3. 보기를 참고하여, 아래 설명에 대한 메서드를 연결지어주세요.

1. 현재 시간까지 경과한 밀리초를 숫자로 변환
2. 1970년 1월 1일 00:00:00를 기점으로 Date 객체의 시간까지 경과된 밀리초 반환
3. 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환
4. 인수로 전달된 값을 기준으로 Date 객체의 시간을 표현한 문자열 반환.

[보기]
a. Date.prototype.toLocalTimeString
b. Date.now
c. Date.prototype.getTime
d. Date.prototype.toTimeString

<br>

## 문제4. 아래는 정규 표현식에서의 플래그입니다. 각 설명에 대한 플래그를 연결지어주세요.

1. 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
2. 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.
3. 대소문자를 구별하지 않고 패턴을 검색한다.

[플래그 종류]
`i / g / m`
