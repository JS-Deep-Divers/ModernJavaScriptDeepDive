### [1] 프로미스 정적 메서드 중 아닌 것은

(1) Promise.resolve

(2) Promise.reject

(3) Promise.all

(4) Promise.get

(5) Promise.allSettled

<details>
<summary>답</summary>
<div markdown="1">

(4)Promise.get  p858

</div>
</details>




### [2] 프로미스의 후속 처리 메서드 3가지는?

<details>
<summary>답</summary>
<div markdown="1">

then, catch, finally

</div>
</details>



### [3] 다음 코드에서 출력되는 코드 순서는?

```jsx
setTimeout(()=>{console.log(1),0}); //(1)

Promise.resolve()
	.then(()=>console.log(2)) //(2)
	.then(()=>console.log(3)); //(3)
```

<details>
<summary>답</summary>
<div markdown="1">

(2) → (3) → (1) p864

</div>
</details>
