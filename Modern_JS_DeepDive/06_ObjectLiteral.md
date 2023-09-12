# 객체 리터럴

- 자바스크립트의 함수는 `일급 객체`이다.
- 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 `메서드(method)`라고 부른다.
- 이처럼 객체는 프로퍼티와 메서드로 구성된 집합체다.

<aside>
💡 인스턴스(Instance)란 클래스에 의해 생성되어 메모리에 저장된 실체를 말한다. 객체지향 프로그래밍에서 객체는 클래스와 인스턴스를 포함한 개념이다. 클래스는 인스턴스를 생성하기 위한 템플릿의 역할을 한다.

</aside>

- 자바스크립트는 `프로토타입 기반 객체 지향` 언어로서 `클래스 기반 객체 지향` 언어와 달리 다양한 객체 생성 방법을 지원한다.
    - `객체 리터럴` , `object 생성자 함수` , `생성자 함수` , `Object.create 메서드` , `클래스(ES6)`
- `객체 리터럴`의 중괄호는 코드 블록을 의미하지 않는다. 코드 블록의 닫는 중괄호 뒤에는 세미 콜론을 붙이지 않는다.
- 하지만 `객체 리터럴` 은 값으로 평가되는 표현식이다. 따라서 세미콜론을 붙인다.
    
    ```jsx
    var person = {
    	name: 'Lee',
    	sayHello: function () {
    		console.log(`Hello! my name is ${this.name}. `);
    	} // 코드 블록을 의미하지 않는다.
    }; // 객체 리터럴을 값으로 평가되는 표현식으므로 세미콜론 붙인다.
    
    console.log(typeof person) // object
    console.log(person) // { name: "Lee", sayHello: f }
    ```
    
- 이미 존재하는 프로퍼티 키를 중복 선언 하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어 쓴다.
    
    ```jsx
    var foo = {
    	name: 'Lee',
    	name: 'kim'
    };
    
    console.log(foo) // {name: 'kim'}
    ```
    

# 프로퍼티 접근

- 마침표 프로퍼티 접근 연산자(.)를 사용하는 `마침표 표기법 dot notation`
- 대괄호 프로퍼티 접근 연산자([ … ])을 사용하는 `대괄호 표기법 bracket notation`

```jsx
var person = {
	name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name) // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']) // Lee 
/* 대괄호 프로퍼티 접근 연산자 내부에 지정하는
 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다. */

console.log(person[name]) // ReferenceError
console.log(person.age) // undefined 객체에 존재하지 않는 프로퍼티에 접근.
```

# 프로퍼티 동적 생성 / 삭제

```jsx
var person = {
	name: 'Lee'
};

person.age = 20 // 프로퍼티 동적 생성

console.log(person) // {name: 'Lee', age: 20}

delete person.age; // age 프로퍼티 삭제
delete person.address // 존재하지 않는 프로퍼티를 삭제하면 무시된다.
```

# ES6에 추가된 객체 리터럴의 확장 기능

## 프로퍼티 축약 표현

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우, 변수 이름과 프로퍼티 키가 동일한 이름일 때 `프로퍼티 키를 생략 property shorthand` 할 수 있다.

```jsx
// ES5
var x = 1, y = 2;

var obj = {
	x: x,
	y: y
}

// ES6
let x = 1, y= 2;

// 프로퍼티 축약 표현
const obj = {x, y} // {x: 1, y: 2}
```

## 계산된 프로퍼티 이름

```jsx
// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
}

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

## 메서드 축약 표현

```jsx
const obj = {
	name: 'Lee',
	// ES5
	sayHi: function() {
		console.log('Hi!' + this.name)
	}
	// 메서드 축약 표현(ES6)
	sayHi() {
		console.log('Hi!' + this.name)
	}
};

obj.sayHi(); // Hi! Lee
```