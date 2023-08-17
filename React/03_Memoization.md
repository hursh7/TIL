# useMemo / useCallback

**useMemo는 특정 결과 값을 재 사용 할 때 쓰고, useCallback은 특정 함수를 재 사용 할 때 사용한다. memo 는 memoization의 약자로, 초기에 render가 된 후 다시 re-render 될 때 불필요한 데이터의 재 호출을 방지하기 위해 캐시로 저장하는 것을 말한다. 그러므로, props나 state의 변경 사항이 빈번한 컴포넌트에서의 사용은 불필요하며, 오히려 메모리 성능의 저하를 불러 올 수 있다.**

1. `React.memo` 단순 UI 컴포넌트의 리 렌더링 방지 (props가 들어가는 순간 리 렌더링 된다)
2. `useMemo` props로 **전달 받은 함수를 실행해서, 그 결과(return) 값을 보존** (의존 인자[deps]가 하나라도 변하면 함수를 다시 실행해서 그 결과 값을 보존)
3. `useCallback` props로 **전달 받은 함수(reference)**를 보존 (의존 인자[deps]가 하나라도 변하면 그에 맞는 새로운 함수를 리턴)

## React.memo

`React.memo`는 **`Higher-Order Components(HOC)`**이다.

(컴포넌트를 인자로 받아서 새로운 컴포넌트를 return해주는 구조의 함수)

- 리액트는 **Shallow copy**를 실행한다. (참조 값만 비교)
  그렇기 때문에 **primitive type**이 아닌 객체,배열,함수와 같은 **reference type**은 같은 참조가 아니라면 새로운 값으로 판단하게 된다.
- 하나의 컴포넌트가 똑같은 props를 넘겨 받았을 때 같은 결과를 렌더링 하고 있다면 `React.memo`를 사용하여 불필요한 컴포넌트 렌더링을 방지 할 수 있다. (이전과 같은 props가 들어올 때는 렌더링 과정을 스킵 하고 가장 최근에 렌더링 된 결과를 재 사용 한다.)
- `React.memo`는 넘겨 받은 props의 변경 여부 만을 체크한다. 하지만 컴포넌트 내부에서 `useState`같은 훅을 사용 하고 있는 경우에는 상태가 변경 되면 리 렌더링 된다.
- props 자체는 primivite type이기 때문에 값만 같아도 되지만, 함수나 객체는 그렇지 않다. 그러므로 `useCallback`, `useMemo`를 통해 특정 props에 deps 를 지정해 렌더링 횟수를 줄일 수 있다.
- 그러나 `useCallback`만으로는 하위 컴포넌트의 리 렌더링을 막을 수 없다. 부모 컴포넌트에서 정의한 `useCallback`은 자식 컴포넌트에서 사용할 때 아무 효력도 없기 때문이다.
  자식 컴포넌트에 `useCallback`에 대한 로직 처리가 없다면, 다시 부모의 렌더링 여부에 따라 렌더링 될 뿐이다.
  즉, 자식 컴포넌트가 **참조 동일성에 의존적인 pureComponent**여야 의미가 있다.

`export default React.memo(component);`

`React.memo` 는 **shouldComponentUpdate**를 내장하고 있어 shallow copy를 실행하여 리렌더링을 방지한다. 이 방식은 같은 **props로 렌더링이 자주 일어나는 컴포넌트**, **렌더링에 리소스 소모가 큰 컴포넌트**에 사용된다.

**React.Memo 활용 예시**

```jsx
function MyComponent(props) {
  // 컴포넌트 로직
}

function areEqual(prevProps, nextProps) {
  // 전달되는 nextProps가 prevProps와 같다면 true를 반환, 같지 않다면 false를 반환해 준다.
}

export default React.memo(MyComponent, areEqual);
```

## useCallback

```jsx
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [x, setX] = useState(0);
  const [y, setY] = useState(0);

  // useCallback 이 () => {console.log(why)} 라는 함수를 반환한다.
  const useCallbackReturn = useCallback(() => {
    console.log(y);
  }, [x]);

  // useCallback 이 담겨있는 함수를 실행
  useCallbackReturn();

  return (
    <>
      <button onClick={() => setX((curr) => curr + 1)}>ButtonX</button>
      <button onClick={() => setY((curr2) => curr2 + 1)}>ButtonY</button>
    </>
  );
}
```

위의 **useCallback** 은 **() => {console.log(y)}** 기능의 함수를 **반환**해주고 다음과 같은 순서로 진행된다.

1. 처음 컴포넌트가 시작될 때 실행 **() => {console.log(0)}**
2. x가 변할 때까지 함수는 **() => {console.log(0)}**
3. **x가 변하면,** y 의 값을 가져와서 **() => {console.log(새로운 값)}**

**ButtonY**를 다섯 번 누를 동안에는 함수가 **() => {console.log(0)}**이다. (이 때 y라는 상태 값은 계속 증가하고 있다.)

그러다가 **ButtonX**를 누르면서 x라는 변수(deps)가 변하자마자, **() => {console.log(5)}** 를 반환 한다. **deps가 변해야 함수 컴포넌트와 상태 값(y)을 공유한다.**

이처럼 useCallback은 함수와 상관 없는 상태 값이 변할 때, 컴포넌트에서의 불필요한 함수 업데이트를 방지해준다. 해당 상황은 `useMemo`에서도 동일하게 동작한다.

```jsx
 useMemo((...)=> fn, deps) === useCallback(fn, deps)
```

useCallback은 deps가 변하면서 함수를 반환할 때, 형태가 같더라도 아예 새로운 함수를 반환한다.

즉, useCallback의 deps가 변할 마다, useCallback은 새로운 무기명 함수를 반환한다.

```jsx
const test1 = () ⇒ {};
const test2 = () ⇒ {};
// 각 변수는 같은 값의 함수를 바라보지만, 전혀 다른 메모리를 가진 변수이다.
```

- **`useMemo`와 `useCallback`을 사용하지 말아야 될 경우**
  - `host` 컴포넌트에 전달하는 모든 항목. 리액트는 여기에 함수 참조가 변경되었는지 신경 쓰지 않기 때문이다.
    (브라우저 또는 모바일에 속하는 플랫폼 컴포넌트 `div`, `span`, `a`, `image`…)
  - `leaf` (DOM에서 다른 컴포넌트를 렝더링 하지 않는 컴포넌트)
  - 의존성 배열에 완전히 새로운 객체와 배열을 전달하지 않는다. 이는 항상 의존성이 같지 않다는 것을 의미하며, 메모이제이션을 하는 의미가 없다.
- **`useMemo`와 `useCallback`을 사용해야 하는 경우**
  - 하위트리에 많은 Consumer가 있는 값을 `Context Provider`에 전달해야 하는 경우 `useMemo` 를 사용하는 것이 좋다.
    ```jsx
    <ProductContext.Provider value={{a, b}} >
    // 어떤 이유에서든 해당 컴포넌트가 리렌더링 된다면 a, b의 값이 동일하더라도
    매번 새로운 참조를 만들어 하위 컴포넌트가 모두 리렌더링 되기 때문에
    useMemo를 사용해 주는 것이 좋다.
    ```
  - 계산 비용이 많이 들고, 사용자의 입력 값이 `map` 과 `filter`를 사용했을 때와 같이 렌더링 이후로도 참조적으로 동일할 가능성이 높은 경우 `useMemo` 를 사용하는 것이 좋다.
  - `ref` 함수를 부수 작용과 함께 전달할 때 `useMemo` 를 사용한다. `ref` 함수가 변경이 있을 때마다, 리액트는 과거 값을 **Null**로 호출하고 새로운 함수를 호출한다. 이 경우 `ref` 함수의 이벤트 리스너를 붙이거나 제거하는 등의 불필요한 작업이 일어날 수 있다.
    예를 들어, `useIntersectionObserver` \*\*\*\*가 반환하는 `ref` 의 경우 콜백 내부에서 observer의 연결이 끊기거나 연결되는 등의 동작이 일어날 수 있다.
  - 자식 컴포넌트에서 `useEffect` 가 반복 적으로 트리거 되는 것을 막고 싶을 때 사용.

`React DevTools Profiler` 를 사용하면 컴포넌트의 리렌더링 속도가 느린 경우, 상태 변경이 일어났을 때 얼마나 렌더링 시간이 걸렸는지 조사할 수 있다. 이를 통해 거대한 계단 식 리 렌더링을 방지하기 위해 `React.memo`를 사용할 위치를 찾을 수 있고, 필요한 경우 `useCallback` `useMemo`를 사용하여 상태변경을 더 효율적으로 만들 수 있다.
