# Array APIS

유용한 배열 함수들 정리. 

### join

배열을 문자열(string)로 반환해 준다. 

```jsx
const fruits = ['apple', 'banana', 'grape'];
const result = fruits.join(); // 구분자를 넣으면 적용됨. 
console.log(result); // apple,banana,orange
const result = fruits.join(', and '); // 구분자를 넣으면 적용됨. 
console.log(result); // apple, and banana, and orange
```

### split

문자열(string)을 배열로 만들어 준다. 

```jsx
const fruits = '🍎', '🍈', '🍇', '🍉';
const result = fruits.split(',', 2); 
// limit을 지정할 수 있다. 2를 지정하면 2개만 배열로 반환한다.
console.log(result); // ['🍎', '🍈']
```

### reverse

배열 순서를 거꾸로 반환한다. 

```jsx
const array = [1, 2, 3, 4, 5];
const result = array.reverse(); 
console.log(result); // [5, 4, 3, 2, 1]
console.log(array); // [5, 4, 3, 2, 1] 배열 자체를 거꾸로 변환시키고 배열 자체를 리턴한다. 
```

### slice

배열의 특정한 부분을 반환한다. (원본 배열은 그대로 보존한다.)

```jsx
const array = [1, 2, 3, 4, 5];
const result = array.slice(2, 5); 
console.log(result); // [3, 4, 5] 2번째 인덱스인 3부터 5번째 인덱스 전인 5까지를 반환한다.
console.log(array); // [1, 2, 3, 4, 5] 배열 자체는 건드리지 않는다. 

```

### splice

배열의 특정한 범위를 삭제하거나 대체 할 수 있고, 새로운 값을 추가 할 수 있다.

(시작 index, 몇 개의 인자를 삭제할 지, 추가할 값)

```jsx
const array = [1, 2, 3, 4, 5];
array.splice(2, 0, -5, -6, -7); // 인덱스 2 부터 -5, -6, -7 추가.
console.log(array); // [1, 2, 3, 4, -5, -6, -7, 4, 5]

array.splice(7, 2, -10, -11); // 인덱스 7 부터 2개의 값을 -10, -11로 대체.
console.log(array); // [1, 2, 3, 4, -5, -6, -7, -10, -11] 
```

### find

배열에서 조건에 맞는 true값을 찾으면 그 값을 반환한다.

```jsx
class Student {
	constructor(name, age, enrolled, score) {
		this.name = name;
		this.age = age;
		this.enrolled = enrolled;
		this.score = score;
	}
}
const students = [
	new Student('A', 29, true, 45),
	new Student('B', 28, false, 80),
	new Student('C', 30, true, 90),
	new Student('D', 40, false, 66),
	new Student('E', 18, true, 88),
];

const result = students.find((student) => student.score === 90); 
// 점수가 90점인 students 배열의 값을 찾고 새로운 class Student로 반환한다. 
console.log(result); // Student {name: "C", age: 30, enrolled: true, score: 90}

```

### filter

배열에서 조건에 맞는 true값을 찾아서 추출한 후, 그 데이터로 새로운 배열을 만든다. 

```jsx
class Student {
	constructor(name, age, enrolled, score) {
		this.name = name;
		this.age = age;
		this.enrolled = enrolled;
		this.score = score;
	}
}
const students = [
	new Student('A', 29, true, 45),
	new Student('B', 28, false, 80),
	new Student('C', 30, true, 90),
	new Student('D', 40, false, 66),
	new Student('E', 18, true, 88),
];

const result = students.filter((student) => student.enrolled);
// students 배열에서 enrolled가 true인 값만 추출한 새로운 배열을 만든다.  
console.log(result); 
// [Student, Student, Student]
// Student {name: "A", age: 29, enrolled: true, score: 45}
// Student {name: "C", age: 30, enrolled: true, score: 90}
// Student {name: "E", age: 18, enrolled: true, score: 88}
```

 

### map

기존 배열의 데이터들이 콜백함수의 인자로 전달되고, 콜백함수가 리턴 하는 값으로 새로운 배열을 만들어서 반환한다.

```jsx
class Student {
	constructor(name, age, enrolled, score) {
		this.name = name;
		this.age = age;
		this.enrolled = enrolled;
		this.score = score;
	}
}
const students = [
	new Student('A', 29, true, 45),
	new Student('B', 28, false, 80),
	new Student('C', 30, true, 90),
	new Student('D', 40, false, 66),
	new Student('E', 18, true, 88),
];

const result = students.map((student) => student.score);
// students 배열의 점수만 반환한다.  
console.log(result); 
// [45, 80, 90, 66, 88]
```

### some

기존 배열의 데이터들이 콜백함수로 리턴이 되는 값이 있는지 확인하여 true 나 false를 반환한다. 

```jsx
class Student {
	constructor(name, age, enrolled, score) {
		this.name = name;
		this.age = age;
		this.enrolled = enrolled;
		this.score = score;
	}
}
const students = [
	new Student('A', 29, true, 45),
	new Student('B', 28, false, 80),
	new Student('C', 30, true, 90),
	new Student('D', 40, false, 66),
	new Student('E', 18, true, 88),
];

const result = students.some((student) => student.score < 50);
// students 배열에서 점수가 50점 미만인 데이터가 있는지 확인한다. 
console.log(result); 
// true
```

### reduce

기존 배열의 데이터들을 시작 점을 지정하여 누적된 값을 구할 때 사용한다.   

```jsx
class Student {
	constructor(name, age, enrolled, score) {
		this.name = name;
		this.age = age;
		this.enrolled = enrolled;
		this.score = score;
	}
}
const students = [
	new Student('A', 29, true, 45),
	new Student('B', 28, false, 80),
	new Student('C', 30, true, 90),
	new Student('D', 40, false, 66),
	new Student('E', 18, true, 88),
];

const result = students.reduce((prev, curr) => prev + curr.score, 0);
// 0부터 시작해서 prev + curr의 누적점수가 리턴된다. 0 -> 45 -> 45+80 -> 125+90 -> 215... 
console.log(result); 
// 369
console.log(result / students.length) // 점수의 평균 구하기
// 73.8
```

### sort

 이전 값 - 현재 값을 했을 때, 마이너스가 나오면 이전 값이 현재 값보다 작다고 계산해서 정렬한다.

```jsx
class Student {
	constructor(name, age, enrolled, score) {
		this.name = name;
		this.age = age;
		this.enrolled = enrolled;
		this.score = score;
	}
}
const students = [
	new Student('A', 29, true, 45),
	new Student('B', 28, false, 80),
	new Student('C', 30, true, 90),
	new Student('D', 40, false, 66),
	new Student('E', 18, true, 88),
];

const result = students.map((student) => student.score) // students 배열의 점수만 반환한다.  
.sort((a, b) => a - b) // 음수가 나오면 a에 b보다 작은 값을 정렬한다. 
.join(); //string 으로 변환

console.log(result); 
// [45, 66, 80, 88, 90]
```