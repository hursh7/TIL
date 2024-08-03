# Alias

```tsx
// Type Aliases : 새로운 타입을 정의할 수 있다.

type Text = string; // Text 라는 새로운 타입을 정의했다.
// 문자열만 입력할 수 있다.
const name: Text = "jun";
const address: Text = "korea";

type Num = number;

type Student = {
  name: string;
  age: number;
};

const student: Student = {
  name: "jun",
  age: 20,
};

// String Literal Types
// type에 문자열을 할당하면 해당 문자열만 입력이 가능하게 한다.
// 보통 TypeScript의 Union 타입에 사용하기 위해 지정한다.
type Name = "name";
let myName: Name;
myName = "name"; // 'name'이라는 문자열만 할당 가능

type JSON = "json";
const json: JSON = "json"; // 'json'이라는 문자열만 할당 가능

// boolean 같은 다른 타입도 지정할 수 있다.
type Boal = true;
```
