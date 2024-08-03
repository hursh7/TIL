# Inference (타입 추론)

```tsx
// Type Inference 변수를 선언함과 동시에 값을 할당하면 해당 값의 타입으로 추론한다.
let text = "hello";
text = 1; // string 아니기 때문에 에러 발생함.

function print(message = "hello") {
  console.log(message);
}
print("hello");

function add(x: number, y: number): number {
  // 타입 추론 보다는 타입을 정확히 명시해주는 게 좋다.
  return x + y;
}
const result = add(1, 2);
```
