# UseReducer

useState를 대체해 복잡한 상태 관리를 위한 hook.

```jsx
import React, { useReducer } from "react";
const [state, dispatch] = useReducer(reducer, initialState, init);
```

**state**

- 컴포넌트에서 사용할 상태

**dispatch 함수**

- 첫번째 인자인 reducer 함수를 실행시킨다.
- 컴포넌트 내에서 state의 업데이트를 일으키키 위해 사용하는 함수

**reducer 함수**

- 컴포넌트 외부에서 state를 업데이트 하는 함수
- 현재state, action 객체를 인자로 받아, 기존의 state를 대체하여 새로운 state를 반환하는 함수

**initialState**

- 초기 state

**init**

- 초기 함수 (초기 state를 조금 지연해서 생성하기 위해 사용)

```jsx
import * as React from "react";
import { useReducer, useState } from "react";
import "./style.css";

// reducer - state를 업데이트 하는 역할 (은행)
// dispatch - state 업데이트를 위한 요구
// action - 요구의 내용

const ACTION_TYPES = {
  deposit: "dposit",
  withdraw: "withdraw",
};

const reducer = (state, action) => {
  switch (action.type) {
    case ACTION_TYPES.deposit:
      return state + action.payload;
    case ACTION_TYPES.withdraw:
      return state - action.payload;
    default:
      return state;
  }
};

function App() {
  const [number, setNumber] = useState(0);
  const [money, dispatch] = useReducer(reducer, 0);

  return (
    <div>
      <p>잔고: {money}원</p>
      <input
        type="number"
        value={number}
        onChange={(e) => setNumber(parseInt(e.target.value))}
        step="1000"
      />
      <button
        onClick={() =>
          dispatch({ type: ACTION_TYPES.deposit, payload: number })
        }
      >
        예금
      </button>
      <button
        onClick={() =>
          dispatch({ type: ACTION_TYPES.withdraw, payload: number })
        }
      >
        출금
      </button>
    </div>
  );
}
```

### Immer 라이브러리

useReducer를 더 편하게 사용할 수 있게 해주는 라이브러리.

```jsx
yarn add immer use-immer
```
