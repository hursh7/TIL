# Interface

```tsx
type PositionType = {
  x: number;
  y: number;
};

interface PositionInterface {
  x: number;
  y: number;
}

// type과 interface 모두 object 형태로 만들 수 있다.
const obj1: PositionType = {
  x: 1,
  y: 1,
};

const obj2: PositionInterface = {
  x: 1,
  y: 1,
};

// type과 interface 모두 Class 형태로 만들 수 있다.
class Pos1 implements PositionType {
  x: number;
  y: number;
}

class Pos2 implements PositionInterface {
  x: number;
  y: number;
}

// Extands : interface는 상속을 통해서 확장이 가능하다.
interface ZPositionInterface extends PositionInterface {
  z: number;
}

// intersection : type은 intersection을 통해 확장 한다.
type ZPositionType = PositionType & { z: number };

// interface만 merge가 가능하다.
// PositionInterface 이라는 동일한 이름으로 한번 더 선언하면 합성이 된다.
interface PositionInterface {
  z: number;
}

const obj3: PositionInterface = {
  x: 1,
  y: 1,
  z: 1, // 모두 사용해야 한다.
};

// Type aliases can use computed properties
type Person = {
  name: string;
  age: number;
};

type Name = Person["name"]; // string

type NumberType = number;
// union type은 type 만 가능하다.
type Direction = "left" | "right";
```
