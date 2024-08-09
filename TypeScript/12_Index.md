# Index

```tsx
{
  const obj = {
    name: "jun",
  };
  obj.name; // jun
  obj["name"]; // jun

  type Animal = {
    name: string;
    age: number;
    gender: "male" | "female";
  };

  type Name = Animal["name"]; // Name의 타입은 string이 된다.
  const text: Name = "text";

  type Gender = Animal["gender"]; // 'male' | 'female'

  type Keys = keyof Animal; // 'name' | 'age' | 'gender'

  type Person = {
    name: string;
    gender: Animal["gender"];
  };

  const person: Person = {
    name: "jun",
    gender: "male",
  };
}
```
