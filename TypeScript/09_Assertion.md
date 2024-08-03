# assertion (타입 강요)

```tsx
function jsStrFunc(): any {
  return "hello";
}
const result = jsStrFunc();
// result 라는 변수가 문자열 타입인걸 정확히 알고 있다면 타입 강요(as string) 사용 가능하다.
console.log((result as string).length);
console.log((<string>result).length);

const wrong: any = 5;
console.log((wrong as Array<number>).push(1));
// 😱 배열 아닌 wrong에 push 해버리면서 app이 죽어버릴 수 있다.

function findNumbers(): number[] | undefined {
  return undefined;
}

// !를 붙여서 타입을 강요할 수 도 있다.
const numbers = findNumbers()!;
numbers!.push(2); // 😱

const button = document.querySelector("class")!;
```
