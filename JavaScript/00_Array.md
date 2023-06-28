# Array(배열)

### 모든 배열 출력하기

```jsx
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
} // for문을 이용한 모든 배열 출력하기.

for (let fruit of fruits) {
  console.log(fruit);
} // for of 를 이용한 모든 배열 출력하기.

fruits.forEach((fruit, index, array) => console.log(fruit, index, array));
// forEach 함수를 이용한 모든 배열 출력하기 index와 배열 전체도 콜백으로 받아 출력할 수 있다.
```

### 배열에 추가/삭제하기

```jsx
fruits.push("🍉", "🍈"); // 배열의 가장 뒤에 데이터를 추가해준다. ['🍎', '🍌','🍉','🍈']
fruits.pop(); // 배열의 가장 뒤에 있는 데이터를 제거한다. ['🍎', '🍌', '🍉']
fruits.unshift("🍈"); // 배열의 가장 앞에 데이터를 추가해준다. ['🍈','🍎', '🍌', '🍉']
fruits.shift(); // 배열의 가장 앞에 있는 데이터를 제거한다. ['🍎', '🍌', '🍉']
```

shift 와 unshift 는 **배열의 앞에 있는 데이터를 조작하여 인덱스 순서를 바꾸기 때문에** pop과 push 보다 **동작이 매우 느리다.**

### splice

```jsx
fruits.splice(시작 index, 제거 할 index 개수); ['🍎', '🍌', '🍉']
fruits.splice(1, 1); // 첫번째 index에서 1개만 지운다. ['🍎', '🍉']
fruits.splice(1, 1, '🍈', '🍇');
// 첫번째 index인 '🍌'를 지우고 그 자리 부터 '🍈', '🍇'를 추가한다.
['🍎', '🍈', '🍇', '🍉']
```

### indexOf / includes

```jsx
const fruits = ['🍎', '🍌'];

console.log(fruits.indexOf('🍎'));
// '🍎'의 데이터가 몇번째 index인지 반환하므로 0 이 출력됨.

console.log(fruits.indexOf('🥥'));
// 배열에 없는 데이터를 출력하면 -1 이 출렴됨.

console.log(fruits.indexOf('🍌', 1)
// 찾을 위치를 두 번째 인자에 부여하면 시작 위치부터 문자열을 찾는다.

console.log(fruits.includes('🍎'));
// includes는 해당 데이터를 가지고 있으면 true 없으면 false를 반환.
// fruits 배열에 '🍎'가 있으므로 true 출력.
```

### search

search 함수는 indexOf 함수와 동일하게 문자열을 찾을 수 있다.
(문자열을 찾을 때 시작 위치는 지정할 수는 없다.)

```jsx
const str = "HTML,CSS,JavaScript";
const res1 = str.search("JavaScript");
// 결과 : 9

const res2 = str.search("React");
// 결과 : -1

/* 한글 찾기 */
const str = "HTML,CSS,자바스크립트";
const kor = str.search(/[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/);
// 결과 : 9

/* 대소문자 구분없이 검색 */
const str = "HTML,CSS,JavaScript";
const res = str.search(/javascript/gi);
// 결과 : 9
```

### lastIndexOf

```jsx
const fruits = ["🍎", "🍌", "🍎"];
console.log(fruits.indexOf("🍎")); // 0
console.log(fruits.lastIndexOf("🍎")); // 2
```

**indexOf** 는 중복되는 '🍎' 중 가장 **첫 번째 값을 반환**한다. 그러므로 0 과 2 중에 **0**을 출력한다.

하지만 **lastIndexOf**는 **제일 마지막 데이터의 값을 반환**해서 **2**를 출력한다.
