# Form

`<form>` 의 `input` 데이터는 사용자가 바로 수정하고 값을 확인 할 수 있기 때문에, 

**`Uncontrolled Components`** 라고 한다. 이는 항상 상태로부터 UI의 업데이트가 이뤄 져야 하는 리엑트의 철학과 맞지 않기 때문에 상태로 관리 하는 방식으로 코드를 작성해주는 것이 좋다.

```jsx
import React, { useState } from 'react';

export default function AppForm() {
  const [form, setFrom] = useState({ name: '', email: '' });
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(form);
  };
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFrom({ ...form, [name]: value });
  };
  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor='name'>이름:</label>
      <input
        type='text'
        id='name'
        name='name'
        value={form.name}
        onChange={handleChange}
      />
      <label htmlFor='email'>이메일:</label>
      <input
        type='email'
        id='email'
        name='email'
        value={form.email}
        onChange={handleChange}
      />
      <button>Submit</button>
    </form>
  );
}
```

**useInput (useHooks)**

```jsx
import { useState, useCallback } from "react";

const useInput = (initialValue) => {
  const [value, setValue] = useState(initialValue);

  const onChange = useCallback((e) => {
    setValue(e.target.value);
  }, []);

  return [value, setValue, onChange];
};

export default useInput;
```