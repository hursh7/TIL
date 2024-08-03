# Basic

### JavaScript의 타입

- `Primitive` : number, string, boolean, bigint, symbol, null, undefined
- `Object` : function, array.....

### TypeScript의 타입

```tsx
// number
const num: number = -6;

// string
const str: string = "hello";

// boolean
const boal: boolean = false;

// undefined
let name: undefined; // 💩 단독으로 쓰는 경우는 없음.
let age: number | undefined; // 보통 Union 타입으로 많이 사용됨.
age = undefined;
age = 1;
function find(): number | undefined {
  return undefined;
}

// null
let person: null; // 💩 단독으로 쓰는 경우는 없음.
let person2: string | null; // 보통 Union 타입으로 많이 사용됨.

// unknown 💩 지양해야 함.
let notSure: unknown = 0;
notSure = "he";
notSure = true;

// any 💩 지양해야 함.
let anything: any = 0;
anything = "hello";

// void
function print(): void {
  console.log("hello");
  return;
}
let unusable: void = undefined; // 💩 변수에 선언하는 경우는 지양.

// never : 리턴하는 값이 절대로 없을 때
function throwError(message: string): never {
  // message -> server (log)
  throw new Error(message);
}
let neverEnding: never; // 💩 변수에 선언하는 경우는 없다.

// object
let obj: object; // 💩 배열도 담을 수 있는 광범위한 타입이라 지양됨.
function acceptSomeObject(obj: object) {}
acceptSomeObject({ name: "jun" });
acceptSomeObject({ animal: "dog" });
```
