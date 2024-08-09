# Utility

### condition

```tsx
type Check<T> = T extends string ? boolean : number;
type Type = Check<string>; // boolean
```

### Readonly

```tsx
type ToDo = {
  title: string;
  description: string;
};

function display(todo: Readonly<ToDo>) {
  // todo.title = 'jaja';
}
```

### Partial

```tsx
type ToDo = {
  title: string;
  description: string;
  label: string;
  priority: "high" | "low";
};

function updateTodo(todo: ToDo, fieldsToUpdate: Partial<ToDo>): ToDo {
  return { ...todo, ...fieldsToUpdate };
}
const todo: ToDo = {
  title: "learn TypeScript",
  description: "study hard",
  label: "study",
  priority: "high",
};

const updated = updateTodo(todo, { priority: "low" });
console.log(updated);
```

### Pick

```tsx
type Video = {
  id: string;
  title: string;
  url: string;
  data: string;
};

type VideoMetaData = Pick<Video, "id" | "title">; // Video 타입에서 id와 title만 쓴다.

function getVideo(id: string): Video {
  return {
    id,
    title: "video",
    url: "https://..",
    data: "byte-data..",
  };
}

function getVideoMetadata(id: string): VideoMetaData {
  return {
    id,
    title: "video",
  };
}
```

### Omit

```tsx
type Video = {
  id: string;
  title: string;
  url: string;
  data: string;
};

type VideoMetaData = Omit<Video, "url" | "data">; // url과 data를 제외한다.(Pick과 반대)

function getVideo(id: string): Video {
  return {
    id,
    title: "video",
    url: "https://..",
    data: "byte-data..",
  };
}

function getVideoMetadata(id: string): VideoMetaData {
  return {
    id,
    title: "video",
  };
}
```

### Record

```tsx
type PageInfo = {
  title: string;
};
type Page = "home" | "about" | "contact";

const nav: Record<Page, PageInfo> = {
  // Page의 요소를 key로, PageInfo를 value로 삼는다.
  home: { title: "Home" },
  about: { title: "About" },
  contact: { title: "Contact" },
};
```
