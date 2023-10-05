# this

this 키워드

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

동작을 나타내는 메서드는 자신이 속한 객체의 상태(프로퍼티)를 참조하고 변경할 수 있어야 한다.

이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```jsx
const circle = {
	// 프로퍼티: 객체 고유의 상태 데이터
	radius: 5,
	// 메서드: 상태 데이터를 참조하고 조작하는 동작
	getDiameter() {
		// 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
		// 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
		return 2 * circle.radius;
	}
};

console.log(circle.getDiameter());  // 10
```

`getDiameter` 메서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 `circle`을 참조하고 있다.

이 참조 표현식이 평가되는 시점은 `getDiameter` 메서드가 호출되어 함수 몸체가 실행되는 시점이다.

위 예제의 객체 리터럴은 `circle` 변수에 할당되기 직전에 평가된다. 따라서 `getDiameter` 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 `circle` 식별자에 생성된 객체가 할당된 이후다.

따라서 메서드 내부에서 `circle` 식별자를 참조할 수 있다.

하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지도 않다. 생성자 함수 방식으로 인스턴스를 생성하는 경우를 생각해보자.

```jsx
function Circle() {
	// 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
	????.radius = radius;
}

Circle.prototype.getDiameter = function() {
	// 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
	return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```

생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야한다.하지만 생성자 함수에 의한 객체 생성 방식은 먼저 생성자 함수를 정의한 이후 `new` 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다. 

→ 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다. 이를 위해 자바스크립트는 `this`라는 특수한 식별자를 제공한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 `arguments` 객체와 `this`가 암묵적으로 함수 내부에 전달된다. 함수 내부에서 `arguments` 객체를 지역 변수처럼 사용할 수 있는 것처럼 `this`도 지역 변수처럼 사용할 수 있다.

단, this가 가리키는 값(this 바인딩)은 함수 호출 방식에 의해 동적으로 결정된다.

🌟 this 바인딩

바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. `this` 바인딩은 `this`(키워드로 분류되지만 식별자 역할을 한다)와 `this`가 가리킬 객체를 바인딩하는 것이다.

위에서 살펴본 객체 리터럴과 생성자 함수의 예제를 `this`를 사용해 수정해보면

```jsx
// 객체 리터럴
const circle = {
	radius: 5,
	getDiameter() {
		// this는 메서드를 호출한 객체를 가리킨다.
		return 2 * circle.radius;
	}
};

console.log(circle.getDiameter());  // 10
```

객체 리터럴의 메서드 내부에서의 `this`는 메서드를 호출한 객체, `circle`을 가리킨다.

```jsx
// 생성자 함수
function Circle(radius) {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
}

Circle.prototype.getDiameter = function () {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter());  // 10
```

생성자 함수 내부의 `this`는 생성자 함수가 생성할 인스턴스를 가리킨다. 이처럼 this는 상황에 따라 가리키는 대상이 다르다.

자바나 C++같은 클래스 기반 언어에서 this는 언제나 클래스가 생성하는 인스턴스를 가리킨다.

하지만 자바스크립트의 `this`는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다. 또한 **strict mode** 역시 `this` 바인딩에 영향을 준다.

this는 코드 어디에서든 참조 가능하다. 전역에서도 함수 내부에서도 참조할 수 있다.

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this);  // window

function sqare(number) {
	// 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
	console.log(this);  // window
	return number * number;
}
sqare(2);

const person= {
	name: 'Lee',
	getName() {
		// 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
		console.log(this);  // {name: 'Lee', getName: *f}*
		return this.name;
	}
};
console.log(person.getName());  // Lee

function Person(name) {
	this.name = name;
	// 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	console.log(this);  // Person {name: "Lee"}
}

const me = new Person('Lee');
```

하지만 `this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의메서드 내부 또는 생성자 함수 내부에서만 의미가 있다. 따라서 `strict mode`가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다. 일반 함수 내부에서 `this`를 사용할 필요가 없기 때문이다.

함수 호출 방식과 this 바인딩

this 바인딩(this에 바인딩될 값)은 함수 호출 방식(함수가 어떻게 호출되었는지)에 따라 동적으로 결정된다.\

🌟 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.

함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this바인딩은 함수 호출 시점에 결정된다.

주의할 것은 동일한 함수도 다양한 방식으로 호출할 수 있다는 것이다. 함수를 호출하는 방식은 다음과 같이 다양하다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```jsx
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
	console.dir(this);
}

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo();  // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo();  // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo();  // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar);    // bar
foo.apply(bar);   // bar
foo.bind(bar)();  // bar
```

함수 호출 방식에 따라 this 바인딩이 어떻게 결정되는지 알아보자

22.2.1 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩된다.

```jsx
function foo() {
	console.log('foo is this: ', this);    // window

	function bar() {
		console.log("bar's this: ", this);   // window
	}
	bar();
}

foo();
```

위 예제처럼 전역 함수는 물론이고 중첩 함수를 일반 함수로 호출하면 함수 내부의 `this`에는 전역 객체가 바인딩된다. 다만 `this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서 `this`는 의미가 없다. 따라서 다음 예제처럼 `strict mode`가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다.

```jsx
function foo() {
	'use strict'
	
	console.log("foo's this: ", this);    // undefined
	function bar() {
		console.log("bar's this: ", this);  // undefined
	}
	bar();
}
foo();
```

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 `this`에는 전역 객체가 바인딩된다.

```jsx
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
	value: 100,
	foo() {
		console.log("foo's this: ", this);  // {value: 100, foo: *f*}
		console.log("foo's this.value: ", this.value);  // 100

		// 메서드 내에서 정의한 중첩 함수
		function bar() {
			console.log("bar's this: ", this);  // window
			console.log("bar's this.value: ", this.value);  // 1
		} 
		
		// 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는
		// 전역 객체가 바인딩된다.
		bar();
	}
};

obj.foo();
```

콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 `this`에도 전역 객체가 바인딩된다. 어떠한 함수라도 일반 함수로 호출되면 `this`에 전역 객체가 바인딩된다.

```jsx
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log("foo's this: ", this);  // {value: 100, foo: *f*}
		// 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
		setTimeout( function() {
			console.log("callback's this: ", this);  // window
			console.log("callback's this.value: ", this); // 1
		}, 100);
	}
};

obj.foo();
```

🌟  setTimeout 함수

setTimeout 함수는 두 번째 인수로 전달한 시간(ms)만큼 대기한 다음, 첫 번째 인수로 전달한 콜백 함수를 호출하는 타이머 함수다. 위 예제에서는 100ms를 대기한 다음, 콜백 함수를 호출한다.

이처럼 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 `this`에는 전역 객체가 바인딩된다.

하지만 메서드 내에서 정의한 중첩 함수 또는 메서드에게 전달한 콜백 함수(보조 함수)가 일반 함수로 호출될 때 메서드 내의 중첩 함수 또는 콜백 함수의 `this`가 전역 객체를 바인딩하는 것은 문제가 있다. 중첩 함수 또는 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 하므로 외부 함수의 일부 로직을 대신하는 경우가 대부분이다. 하지만 외부 함수인 메서드와 중첩 함수 또는 콜백 함수의 `this`가 일치하지 않는다는 것은 중첩 함수 또는 콜백 함수를 헬퍼 함수로 동작하기 어렵게 만든다.

위 예제의 경우 메서드 내부에서 `setTimeout` 함수에 전달된 콜백 함수의 `this`에는 전역 객체가 바인딩된다.

따라서 `this.value`는 `obj` 객체의 `value` 프로퍼티가 아닌 전역 객체의 `value` 프로퍼티(window.value)를 참조한다. var 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 되므로 `window.value`는 1이다.

메서드 내부의 중첩 함수나 콜백 함수의 `this` 바인딩을 메서드의 `this` 바인딩과 일치시키기 위한 방법은 다음과 같다.

```jsx
var value = 1;

const obj = {
	value: 100,
	foo() {
		// this 바인딩(obj)을 변수 that에 할당한다.
		const that = this;

		// 콜백 함수 내부에서 this 대신 that을 참조한다
		setTimeout(function () {
			console.log(that.value);  // 100	
		}, 100);
	}
};

obj.foo();
```

위 방법 이외에도 자바스크립트는 `this`를 명시적으로 바인딩할 수 있는 Function.prototype.apply / Function.prototype.call / Function.prototype.bind 메서드를 제공한다.

```jsx
var value = 1;

const obj = {
	value: 100,
	foo() {
		// 콜백 함수에 명시적으로 this를 바인딩한다.
		setTimeout(function () {
			console.log(this.value);  // 100
		},bind(this), 100);
	}
};

obj.foo();
```

또는 화살표 함수를 사용해서 `this` 바인딩을 일치시킬 수도 있다.

```jsx
var value = 1;

const obj = {
	value: 100,
	foo() {
		// 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
		setTimeout(() => console.log(this.value), 100) // 100
	}
};

obj.foo();
```

22.2.2 메서드 호출

메서드 내부의 `this`에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표( . ) 연산자 앞에 기술한 객체가 바인딩된다. 주의할 것은 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

```jsx
const person = {
	name: 'Lee',
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
		return this.name;
	}
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName());  // Lee
```

위 예제의 `getName` 메서드는 `person` 객체의 메서드로 정의되었다. 메서드는 프로퍼티에 바인딩된 함수다. 

→ person 객체의 `getName` 프로퍼티가 가리키는 함수 객체는 `person` 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다. `getName` 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6814023b-0541-4cd9-a222-c1fe51dfd385/Untitled.png)

따라서 `getName` 프로퍼티가 가리키는 함수 객체

→ `getName` 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```jsx
const anotherPerson = {
	name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName());  // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName());  // ''
// 일반 함수로 호출된 getName 함수 내부의 
// this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```

따라서 메서드 내부의 `this`는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/025eeab9-be05-4050-ba20-ffc2d9addd14/Untitled.png)

프로토타입 메서드 내부에서 사용된 `this`도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function () {
	return this.name;
}

const me = new Person('Lee');

// getName 메서드를 호출한 객체를 me다.
console.log(me.getName());  // ⓵ Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName());  // ⓶ Kim
```

⓶의 경우 getName 메서드를 호출한 객체는 Person.prototype이다. `Person.prototype`도 객체이므로 직접 메서드를 호출할 수 있다. 따라서 `getName` 메서드 내부의 this는 Person.prototype을 가리키며 this.name은 ‘Kim’이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b893a7a1-2273-4ba8-9ecb-a6f6fce67db9/Untitled.png)

22.2.3 생성자 함수 호출

생성자 함수 내부의 `this`에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

```jsx
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}; 
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter());  // 10
console.log(circle2.getDiameter());  // 20
```

생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수다. 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new` 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 만약 `new` 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```jsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3);  // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius);  // 15
```

22.2.4 Function.prototype.apply / call / bind 메서드에 의한 간접 호출 

apply, call, bind 메서드는 `Function.prototype`의 메서드다.

→ 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2fd98e15-8461-4e7d-8910-fdac4b28f76e/Untitled.png)

Function.prototype.apply, Function.prototype.call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다. `apply`와 `call` 메서드의 사용법은 다음과 같다.

```jsx
/**
* 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
* @param thisArg - this로 사용할 객체
* @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
* @returns 호출된 함수의 반환값
*/
Function.prototype.apply(thisArg[, argsArray])

/**
* 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
* @param thisArg - this로 사용할 객체
* @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
* @returns 호출된 함수의 반환값
*/
Function.prototype.call (thisArg[, arg1[, arg2[, ...]]])
```

```jsx
function getThisBinding() {
	return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding());  // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg));  // {a: 1}
console.log(getThisBinding.call(thisArg));   // {a: 1}
```

`apply`와 `call` 메서드의 본질적인 기능은 함수를 호출하는 것이다. `apply`와 `call` 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 `this`에 바인딩한다.

`apply`와 `call` 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다. 위 예제는 호출할 함수, 즉, `getThisBinding` 함수에 인수를 전달하지 않는다. `apply`와 `call` 메서드를 통해 `getThisBinding` 함수를 호출하면서 인수를 전달해보자.

```jsx
function getThisBinding() {
	console.log(arguments);
	return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: *f, Symbol*(Symbol.iterator): *f*]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): *f*]
// {a: 1}
```

`apply` 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다. `call` 메서드는 호출한 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다. 이처럼 `apply`와 `call` 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 `this`로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다.

apply와 call 메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다. arguments 객체는 배열이 아니기 때문에 Array.prototype.slice 같은 배열의 메서드를 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다. 

```jsx
function convertArgsToArray() {
	console.log(arguments);

	// arguments 객체를 배열로 변환
	// Array.prototype.slice를 인수 없이 호출하면 배열의 복사본을 생성한다.
	const arr = Array.prototype.slice.call(arguments);
	// const arr = Array.prototype.slice.apply(arguments);

	console.log(arr);

	return arr;
}

convertArgsToArray(1, 2, 3);  // [ 1, 2, 3 ]
```

Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
function getThisBindiing() {
	return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)());  // {a:1}
```

bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 이해 유용하게 사용된다.

```jsx
const person = {
	name: 'Lee',
	foo(callback) {
		// ⓵
		setTimeout(callback, 100);
	}
};

person.foo(function () {
	console.log(`Hi! my name is ${this.name}`);  // ⓶ Hi! my name is .
	// 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
	// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
	// Node.js 환경에서 this.name은 undefined다.
});
```

[person.foo](http://person.foo)의 콜백 함수가 호출되기 이전인 ⓵의 시점에서 this는 foo 메서드를 호출한 객체(person 객체)를 가리킨다. 그러나 person.foo의 콜백 함수가 일반 함수로서 호출된 ⓶의 시점에서 this는 전역 객체 window를 가리킨다. 따라서 person.foo의 콜백 함수 내부에서 this.name은 window.name과 같다.

이때 위 예제에서 person.foo의 콜백 함수는 외부 함수 person.foo를 돕는 헬퍼 함수(보조함수) 역할을 하기 때문에 외부 함수 person.foo 내부의 this와 콜백 함수 내부의 this가 상이하면 문맥상 문제가 발생한다.

따라서 콜백 함수 내부의 this를 외부 함수 내부의 this와 일치시켜야 한다. 이때 bind 메서드를 사용하여 this를 일치시킬 수 있다.

```jsx
const person = {
	name: 'Lee',
	foo(callback) {
		// bind 메서드로 callback 함수 내부의 this 바인딩을 전달
		setTimeout(callback.bind(this), 100);
	}
};

person.foo(function() {
	console.log(`Hi! my name is ${this.name}`);  // Hi! my name is Lee
})
```

지금까지 함수 호출 방식에 따라 this 바인딩이 동적으로 결정되는 것에 대해 살펴보았다.

| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체 |