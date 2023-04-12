# 26. 함수의 추가 기능



## **[1] 화살표 함수는 중복된 매개변수의 이름을 선언할 수 없다 O, X**

<details>
  <summary> 정답 <summary>
<div markdown="1">
  X
    </div>
</details>



## **[2] 다음 코드의 출력 내용은?**

```javascript
const counter = {
	num:1,
	increase: () => ++this.num
};
console.log(counter.increase());
```

<details>
  <summary> 정답 <summary>
    NaN
</details>



## **[3] 화살표 함수 내부에서 this를 참조하면 어떻게 되는지 서술하세요**

<details>
  <summary> 정답 <summary>
    가장 가까운 상위 함수중에서 화살표 함수가 아닌 this를 참조한다.
상위 스코프의 this를 그대로 참조한다.
</details>

