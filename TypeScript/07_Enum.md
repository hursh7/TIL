# Enum

```tsx
// Enum: 상수 값을 한곳에 모아서 정리해놓는 타입

// JavaScript: Enum이 제공되지 않는다.
const MAX_NUM = 6;
const MAX_STUDENTS_PER_CLASS = 10;
const MONDAY = 0;
const TUESDAY = 1;
const WEDNESDAY = 2;
// 각각 상수를 지정할 수 있지만 Enum이 제공되지 않기 때문에 Object의 freeze API를 사용한다.
const DAYS_ENUM = Object.freeze({ MONDAY: 0, TUESDAY: 1, WEDNESDAY: 2 });
const dayOfToday = DAYS_ENUM.MONDAY;

// TypeScript
type DaysOfWeek = "Monday" | "Tuesday" | "Wednesday"; // 앞글자만 대문자 나머지는 소문자로 작성
enum Days {
  Monday = 1, // 숫자를 지정하면 다음값 부터 자동으로 숫자 지정됨 (기본값 0)
  Tuesday, // 2
  Wednesday, // 3
  Thursday, // 4
  Friday, // 5
  Saturday, // 6
  Sunday, // 7
}
console.log(Days.Monday);
let day: Days = Days.Saturday;
day = Days.Tuesday;
day = 10; // enum에 없기 때문에 에러 발생 (5.0 버전부터 적용됨.)
console.log(day);

let dayOfweek: DaysOfWeek = "Monday";
dayOfweek = "Wednesday";

// 보통 Enum 보다는 Union 타입을 사용하는 것이 권장된다.
```
