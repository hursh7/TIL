# Map

```tsx
type Video = {
  title: string;
  author: string;
  description: string;
};

type Optional<T> = {
  [P in keyof T]?: T[P]; // for...in
  // type의 key를 모두 순회해서 value 값을 출력 (optinal로)
};

// 이런 형태가 된다.
type Optional = {
  title?: string;
  author?: string;
  description?: string;
};

type ReadOnly<T> = {
  readonly [P in keyof T]: T[P];
};

type VideoOptional = Optional<Video>;

const videoOptional: VideoOptional = {
  title: "hi",
};

// typesript 문서 Proxy 예제도 map 타입을 활용함.
type Proxy<T> = {
  get(): T;
  set(value: T): void;
};

type Proxify<T> = {
  [P in keyof T]: Proxy<T[P]>;
};
```
