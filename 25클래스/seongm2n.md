### 25

[25.1] 클래스는 프로토타입의 문법적 설탕인가?

😤 자바스크립트는 프로토타입 기반 객체지향 언어

😤 아주 강력한 객체지향 프로그래밍 능력을 가짐

😤 ES6에서 도입된 클래스는 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 매커니즘을 제시

새롭게 클래스 기반 객체지향 모델을 제공하는 것은 아님

😤 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕이라 볼 수 있음

❗클래스는 생성자 함수와 매우 유사하게 동작하지만 몇가지 차이가 있음

| 클래스 | 생성자 함수 |
| --- | --- |
| 클래스를 new 연산자 없이 호출하면 에러 발생 | 생성자 함수를 new연산자 없이 호출하면 일반 함수로서 호출 |
| 상속을 지원하는 extends와 super키워드 제공 | extends와 super키워드 지원x |
| 호이스팅이 발생하지 않는 것처럼 동작 | 함수 선언문으로 정의된 생성자 함수는 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생 |
| 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없음 | 암묵적으로 strict mode가 지정되지 않음 |
| contructor, 프로토타입 매서드, 정적 메서드는 모두 프로퍼티 어트리뷰트[[Enumerable]]의 값이 false, 열거되지 않음 |  |

```jsx
// 생성자 함수 정의 방식
var Person = (function(){
	// 생성자 함수
	function Person(name){
		this.name = name;
	}

	// 프로토타입 메서드
	Person.prototype.sayHi = function(){
		console.log('Hi! My name is '+this.name);
	};

	// 정적 메서드
	Person.sayHello = function(){
		console.log('Hello!');
	};

	// 생성자 함수 반환
	return Person;
}());
```

```jsx
// class 정의 방식
class Person{
	// 생성자
	constructor(name) {
		this.name = name;
	}

	// 프로토타입 메서드
	sayHi() {
		console.log(`Hi! My name is ${this.name}`);
	}

	// 정적 메서드
	static sayHello() {
		console.log('Hello!');
	}
}
```

❗클래스와 생성자 함수의 유사점

- 프로토타입 기반의 객체지향을 구현했다는 점에서 매우 유사

[25.2] 클래스 정의

- 클래스는 class 키워드 사용

```jsx
//클래스 선언문
class Person{}

//익명 클래스 표현식
const Person = class {};

//기명 클래스 표현식
const Person = class Myclass{};
```

- 클래스를 표현식으로 정의할 수 있다는 것 → 클래스가 값으로 사용할 수 있는 일급 객체라는 것을 의미
- 클래스는 일급 객체로서 특징을 갖음
    - 무명의 리터럴로 생성할 수 있음, 즉 런타임에 생성이 가능
    - 변수나 자료구조(객체,배열 등)에 저장
    - 함수의 매개변수에게 전달
    - 함수의 반환값으로 사용

---

- 클래스 몸체에서 정의할 수 잇는 메서드
    - constructor(생성자)
    - 프로토타입 메서드
    - 정적 메서드

    ```jsx
    // 클래스 선언문
    class Person{
    	// 생성자
    	constructor(name){
    		// 인스턴스 생성 및 초기화
    		this.name = name; //name 프로퍼티는 public함
    	}
    	sayHi(){
    		console.log(`Hi! My name is ${this.name}`);
    	}

    	// 정적 메서드
    	static sayHello(){
    		console.log('Hello!);
    	}
    }

    // 인스턴스 생성
    const me = new Person('Lee');

    // 인스턴스의 프로퍼티 참조
    console.log(me.name); // Lee
    // 프로토타입 메서드 호출
    me.sayHi(); // Hi! My name is Lee
    //정적 메서드 호출
    Person.sayHello(); // Hello!
    ```


[25.3] 클래스 호이스팅

- 클래스는 함수로 평가
- 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생
- 단, 클래스는 let,const키워드로 선언한 변수처럼 호이스팅
    - var,let,const,function,function*,class키워드를 사용하여 선언된 모든 식별자는 호이스팅
    - 모든 선언문은 런타임 이전에 먼저 실행되기 때문

[25.4] 인스턴스 생성

- 생성자 함수이며 new연산자와 함께 호출되어 인스턴스 생성

```jsx
class Person{}

// 인스턴스 생성
const me = new Person(); //new연산자 없이 호출하면 타입 에러 발생
console.log(me); // person{}
```

[25.5] 메서드

- 클래스 몸체에는 0개 이상의 메서드만 선언
- 클래스 몸체에서 정의할 수 있는 메서드
    - constructor(생성자)
    - 프로토타입 메서드
    - 정적 메서드

(1) contructor

- 인스턴스를 생성하고 초기화하기 위한 특수한 메서드
- constructor이라는 이름을 변경할 수 없음

```jsx
class Person{
	//생성자
	constructor(name){
	//인스턴스 생성 및 초기화
	this.name = name;
	}
}
```

- constructor 생략 가능 → 빈 constructor가 암묵적으로 정의
- 인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초깃값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초깃값을 전달
- 이때 초깃값은 constructor의 매개변수에게 전달

```jsx
//constructor내에서 인스턴스 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스 초기화를 실행,
//따라서 인스턴스를 초기화하려면 constructor를 생략해서는 안됌
class Person {
	constructor(name, address){
		// 인수로 인스턴스 초기화
		this.name = name;
		this.address = address;
	}
}

//인수로 초기값을 전달, 초기값은 constructor에 전달
const me = new Person('Lee','Seoul');
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

(2) 프로토타입 메서드

- 생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 명시적으로 프로토타입에 메서드를 추가해야 함

```jsx
// 생성자 함수
function Person(name){
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function() {
	console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
me.sayHi(); //Hi! My name is Lee
```

(3) 정적 메서드

- 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말함
- 정적 메서드는 인스턴스로 호출 할 수 없음
- 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인상에 존재하지 않기 때문
- 인스턴스 프로토타입 체인 상에는 클래스가 존재하지 않기 때문에 인스턴스로 클래스의 메서드를 상속받을 수 없음
- 생성자 함수의 경우 정적 메서드를 생성하기 위해서는 담과 같이 명시적으로 생성자 함수에 메서드를 추가해야함

```jsx
// 생성자 함수
function Person(name){
	this.name = name;
}

// 정적 메서드
Person.sayHi = function() {
	console.log('Hi!');
}

// 정적 메서드 호출
Person.sayHi(); // Hi!
```

- 클래스에서는 메서드에 static키워드를 붙이면 정적 메서드(클래스 메서드)가 됌

```jsx
class Person{
	// 생성자
	constructor(name){
		//인스턴스 생성 및 초기화
		this.name = name;
	}
	//정적 메서드
	static sayHi() {
		console.log('Hi!');
	}
}
```

(4) 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다름
2. 정적 메서드는 클래스 호출하고 프로토타입 메서드는 인스턴스로 호출
3. 정적 메서드는 인스턴스 프로터티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있음

(5) 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없음
3. 암묵적으로 strict mode로 실행
4. for…in문이나 Object.keys 메서드 등으로 열거할 수 없음, 즉 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트의 값이 false
5. 내부 메서드를 갖지 않는 non-constructor, 따라서 new연산자와 함께 호출 할 수 없음

[25.6] 클래스의 인스턴스 생성 과정

- new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드가 호출
- 클래스는 new연산자 없이 호출할 수 없다.
1. 인스턴스 생성과 this 바인딩
    - new 연산자와 함께 클래스를 호출하면 constructor의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체 생성
        - 이 빈 객체(아직 완성은 되지 않았지만)가 바로 클래스가 생성한 인스턴스
    - 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩
2. 인스턴스 초기화
    - constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화
    - this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가
    - constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화
3. 인스턴스 반환
    - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

```jsx
class Person {
	//생성자
	constructor(name){
		//1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
		console.log(this); //Person{}
		console.log(Object.getPrototypeOf(this) === Person.prototype); //true
		//2. this에 바인딩되어 있는 인스턴스를 초기화
		this.name = name;
		//3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
	}
}
```

[25.7] 프로퍼티

(1) 인스턴스 프로퍼티

- constructor 내부에서 정의

    ```jsx
    class Person{
    	constructor(name){
    	//인스턴스 프로퍼티
    	this.name = name; //name 프로퍼티는 public 함
    	}
    }
    const me = new Person('Lee');
    console.log(me); //Person {name:"Lee"}
    console.log(me.name); // Lee
    ```

- ES6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않음
- 따라서 인스턴스 프로퍼티는 언제나 public

(2) 😕접근자 프로퍼티 (get,set)

- 자체적으로 값을 갖지 않고 다른 프로퍼티의 값을 읽거나 저장할 때 사용
- 즉 getter함수와 setter함수로 구성
- getter는 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
    - getter는 메서드 이름 앞에 get키워드를 사용해 정의
    - setter는 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
    - setter는 메러드 이름 앞에 set키워드를 사용해 정의
- getter와 setter

```jsx
const person = {
	firstName: 'Ungmo',
	lastName: 'Lee',

	// fullName은 접근자 함수로 구성된 접근자 프로퍼티
	// getter 함수
	get fullName(){
		return `${this.firstName} ${this.lastName]`;
	};
	//setter 함수
	set fullName(name){
		// 배열 디스트럭처럼 할당: 나중에 36.1 배열 디스트럭처링 할당 참고
		[this.firstName, this.lastName] = name.split(' ');
	}

}
```

- getter와 setter 이름은 인스턴스 프로퍼티처럼 사용
- getter & setter는 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식

| getter | setter |
| --- | --- |
| 이름 그대로 무언가를 취득할 때 사용 반드시 무언가 반환 | 무언가를 프로퍼티에 할당해야 할 떄 사용하므로 반드시 매개변수 있어야 함 |
|  | 단 하나의 값만 할당받기 때문에 단 하나의 매개변수만 선언 |

(3) 클래스 필드 정의 제안

- 클래스 필드(필드 또는 멤버) : 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리킴
- 클래스 필드를 참조하는 경우 자바와 같은 클래스 기반 객체지향 언어에서는 this를 생략할 수 있으니 자바스크립트에서는 this를 반드시 사용
- 클래스 필드에 초깃값을 할당하지 않으면 undefined를 갖음
- 클래스 필드를 초기화할 필요가 있다면 어차피 constructor 내부에서 클래스 필드를 참조하여 초기값을 할당해야 함
- 이때 this, 즉 클래스가 생성한 인스턴스에 클래스 필드에 해당하는 프로퍼티가 없다면 자동 추가
    - 프로퍼티 정의하는 방식 두가지
        1. 인스턴스를 생성할 때 외부 초깃값으로 클래스 필드를 초기화할 필요가 있다면 constructor에서 인스턴스 프로퍼티를 정의하는 기존 방식 사용
        2. 인스턴스를 생성할 때 외부 초깃값으로 클래스 필드를 초기화할 필요가 없다면 기존의 constructor에서 인스턴스 프로퍼티를 정의하는 방식과 클래스 필드 정의 제안 모두 사용

(4) private필드 정의 제안

- private필드의 선두에는 #을 붙임
- private필드 참조할 때도 #을 붙어줌

| 접근 가능성 | public | private |
| --- | --- | --- |
| 클래스 내부 | o | o |
| 자식 클래스 내부 | o | x |
| 클래스 인스턴스를 통한 접근 | o | x |

(5) static 필드 정의 제안

- static 키워드 사용해서 정적 메서드 정의
- 하지만 static 키워드를 사용하여 정적 필드를 정의할 수 없었음

[25.8] 상속에 의한 클래스 확장

(1) 클래스 상속과 생성자 함수 상속

- 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스 확장하여 정의

(2) extends 키워드

- 상속을 통해 클래스를 확장하려면 extends키워드를 사용하여 상속받은 클래스 정의

(3) 동적 상속

- 클래스뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다. 단, extends 키워드 앞에는 반드시 클래스가 와야 함
