# Intersection

```tsx
// Intersection Types: &

type Student = {
  name: string;
  score: number;
};

type Worker = {
  empolyeeId: number;
  work: () => void;
};

function internWork(person: Student & Worker) {
  console.log(person.name, person.empolyeeId, person.work());
}

internWork({
  // Student & Worker Type 중 하나라도 없으면 에러 발생한다.
  name: "jun",
  score: 1,
  empolyeeId: 123,
  work: () => {},
});
```
