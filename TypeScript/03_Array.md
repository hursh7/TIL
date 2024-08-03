# Array

```tsx
// Array
const fruits: string[] = ["🍅", "🍌"];
const scroes: Array<number> = [1, 3, 4];
function printArray(fruits: readonly string[]) {
  // 읽기 모드 적용하려면 Array<...> 타입은 허용되지 않는다.
  fruits.push; // 배열 요소를 수정하려 하면 에러가 발생한다.
}

// Tuple : 서로 다른 타입을 담을 수 있다. -> 사용 권장되지 않는다.
// interface, type alias, class로 대체 해서 사용하는게 좋다.
let student: [string, number];
student = ["name", 123];
student[0]; // name
student[1]; // 123

// 굳이 사용한다면 구조분해할당으로 사용하는게 좋다. (React의 useState도 Tuple 형태이다.)
const [name, age] = student;
```
