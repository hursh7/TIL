# Union

```tsx
// Union Types: OR

type Direction = "left" | "right" | "up" | "down";
function move(direction: Direction) {
  console.log(direction);
}
move("down");

type TileSize = 8 | 16 | 32;
const tile: TileSize = 16;

// function: login -> success, fail
type SuccessState = {
  response: {
    body: string;
  };
};
type FailState = {
  reason: string;
};
type LoginState = SuccessState | FailState;

function login(): LoginState {
  return {
    response: {
      body: "logged in!",
    },
  };
}

// printLoginState(state: LoginState)
// success -> 🎉 body
// fail -> 😭 reason
function printLoginState(state: LoginState) {
  if ("response" in state) {
    console.log(`🎉 ${state.response.body}`);
  } else {
    console.log(`😭 ${state.reason}`);
  }
}
```

# discriminated

```tsx
// discriminated Type 차별화, 구분할 수 있는 타입
// 동일한 키를 가지고 있지만 타입에 따라 다른 값을 가지고 있을 수 있다.
type SuccessState = {
  // result라는 동일한 키에 다른 값을 가짐.
  result: "success";
  response: {
    body: string;
  };
};
type FailState = {
  // result라는 동일한 키에 다른 값을 가짐.
  result: "fail";
  reason: string;
};
type LoginState = SuccessState | FailState;

function login(): LoginState {
  return {
    result: "success",
    response: {
      body: "logged in!",
    },
  };
}

// LoginState 는 SuccessState | FailState; 타입을 가지며 각자 동일한 result 키에 다른 값("success", "fail" 로 접근해서 사용할 수 있다.)
function printLoginState(state: LoginState) {
  if (state.result === "success") {
    console.log(`🎉 ${state.response.body}`);
  } else {
    console.log(`😭 ${state.reason}`);
  }
}
```
