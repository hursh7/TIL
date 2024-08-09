# Generic

### function

```tsx
function checkNotNullBad(arg: number | null): number {
  if (arg == null) {
    throw new Error("not valid number!");
  }
  return arg;
}
// number만 확인할 수 있어서 재사용성이 떨어진다.

function checkNotNullAnyBad(arg: number | null): any {
  if (arg == null) {
    throw new Error("not valid number!");
  }
  return arg;
}
// any를 사용하면 타입을 보장받지 못한다.

function checkNotNull<T>(arg: T | null): T {
  // 제네릭 함수는 보통 T로 사용한다.
  if (arg == null) {
    throw new Error("not valid number!");
  }
  return arg;
}
const number = checkNotNull(123);
const boal: boolean = checkNotNull(true);
// 제네릭을 사용하면 변수에 할당 할 때 타입을 보장 받을 수 있다.
```

### class

```tsx
interface Either<L, R> {
  left: () => L;
  right: () => R;
}

class SimpleEither<L, R> implements Either<L, R> {
  constructor(private leftValue: L, private rightValue: R) {}
  left(): L {
    return this.leftValue;
  }
  right(): R {
    return this.rightValue;
  }
}
const either: Either<number, number> = new SimpleEither(4, 5);
either.left();
either.right();
```

### constraints (강제)

```tsx
interface Employee {
  pay(): void;
}

class FullTimeEmployee implements Employee {
  pay() {
    console.log("full time!");
  }
  workFullTime() {}
}

class PartTimeEmployee implements Employee {
  pay() {
    console.log("part time!");
  }
  workPartTime() {}
}

// 세부적인 타입을 인자로 받아서 추상적인 타입으로 다시 리턴하는 함수는 좋지 않다. 💩
function payBad(employee: Employee): Employee {
  employee.pay();
  return employee;
}

// Generic을 사용하지만 Employee에 있는 함수만 사용할 수 있게 constriants(강제) 한다.
function pay<T extends Employee>(employee: T): T {
  employee.pay();
  return employee;
}

const jun = new FullTimeEmployee();
const min = new PartTimeEmployee();

const junAfterPay = payBad(jun) as FullTimeEmployee; // type assertion을 해야지만 workFullTime 함수 사용 가능하다.
const minAfterPay = payBad(min);
// 둘다 가능
junAfterPay.workFullTime();
junAfterPay.pay();
// 하나만 가능
minAfterPay.pay();

const obj = {
  name: "jun",
  age: 30,
};

// keyof T T오브젝트안에 들어 있는 key의 타입
// T[K] T의 key 의 value(값 자체)
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

console.log(getValue(obj, "name")); // jun
console.log(getValue(obj, "age")); // 30
```
