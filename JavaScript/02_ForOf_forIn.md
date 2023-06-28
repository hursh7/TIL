# for …of Vs for …in

ES6 부터 추가 된 반복문이다. for …in 은 모든 열거 가능한 객체를 반복해주고, for …of는 “컬렉션 전용” 으로 배열(Array)의 반복에 사용된다.

### for …of

- 인덱스 순서가 중요한 배열에서 사용.

### for …in

- **순서를 보장하지 않고 반복** 되기 때문에 배열에서 사용하지 않는 게 권장된다.

```jsx
Object.prototype.objCustom = function () {}; // Object 프로토타입에 커스텀 속성 추가
Array.prototype.arrCustom = function () {};  // Array 프로토타입에 커스텀 속성 추가

let iterable = [3, 5, 7];
iterable.foo = "hello"; // 배열객체에 foo 속성 추가 및 값 할당.

for (let i in iterable) {
  console.log(i); // 0, 1, 2, foo, arrCustom , objCustom 출력
}

for (let i of iterable) {
  console.log(i); // 3, 5, 7 출력
}
```

- iterable이라는 배열을 for.. in으로 반복하는 경우, 눈에 보이는 배열의 원소 뿐만 아니라, 눈에 보이지 않는 프로토타입 속성의 인덱스까지 모두 돌며 반복한다. 게다가 **objCustom**을 먼저 정의해주었지만, **arrCustom**이 먼저 출력 되며 인덱싱의 순서를 보장하지 않는다.

```jsx
const man = {name: 'jun', age: 30, job: 'FrontEnd Developer'}

for(const key in man){
	console.log(key); // name, age, job
}

for(const key in man){
  console.log(man[key]); // jun, 30, FrontEnd Developer
}

for(const key of man){
  console.log(key); // TypeError: man is not iterable
}
```