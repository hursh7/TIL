# 1. useEffect

useEffect은 리액트 컴포넌트가 랜더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 hook이다. 클래스형 컴포넌트의 componentDidMount와 componentDidUpdate를 합친 형태로 봐도 무방하다.

## componentDidMount

useEffect에서 설정한 함수를 컴포넌트가 화면에 처음 랜더링 될 때만 실행하고, 업데이트 때는 실행하지 않으려면 함수의 **두 번째 파라미터로 빈 배열**을 넣어주면 된다.

```jsx
useEffect(() => {
  // 실행 할 로직
  console.log("마운트될 때만 실행.");
}, []);
```

## **componentDidUpdate**

특정값이 변경될 때 호출하고 싶으면 두 번째 파라미터의 배열 안에 검사하고 싶은 값을 전달

```jsx
useEffect(() => {
  // 실행 할 로직
  console.log(test)
}, [test]);}
```

## **cleanUp**

useEffect는 리액트가 랜더링 되고 난 직후 최초 한 번은 실행된다. 이후 두 번째 파라미터 배열에 무엇을 전달 하는 지에 따라 조건이 달라진다. 컴포넌트가 언마운트 되기 전이나 업데이트 되기 직전에 어떠한 작업을 수행하고 싶으면 useEffect에서 뒷정리(cleanup) 함수를 반환해 주어야 한다.

```jsx
seEffect(() => {
  console.log("effect");
  console.log(test);
  return () => {
    console.log("cleanup");
    console.log(test);
  };
}, [test]);
```

랜더링 될 때마다 뒷정리 함수가 실행되는 것을 확인할 수 있다. 그리고 뒷정리 함수가 실행될 때는 업데이트 되기 직전의 값을 보여 준다.

오직 언마운트될 때만 뒷정리 함수를 호출하고 싶으면 두 번째 파라미터에 빈 배열을 전달하면 된다.

# 2. useState

`useState` 는 **비동기로 동작한다.**

_(정확하게 얘기하면 `setState`가 비동기로 동작한다. )_

```jsx
const [numbers, setNumbers] = useState(0);

const Plus = () => {
  setNumbers(numbers + 1);
  setNumbers(numbers + 1);
  setNumbers(numbers + 1);
};

return (
  <div>
    <p>{numbers}</p>
    <button onClick={Plus} />
  </div>
);
```

- 실행하기 전에 예측을 해보면 버튼을 클릭할 때마다 `setNumbers(numbers + 1)`을 세 번 해줌으로써 `numbers`를 3씩 증가 시킬 것 같지만, `3이 아니라 1씩 증가`한다.
- 하나의 페이지나 컴포넌트 내에도 수많은 상태 값이 존재한다. 만약 이 상태 하나하나가 `바뀔 때마다 화면을 리렌더링 한다면 문제`가 생길 수도 있다.
- 때문에 리액트는 성능의 향상을 위해서 `setState` 를 연속 호출하면 배치 처리하여 한 번에 렌더링 하도록 하였다. 아무리 많은 `setState`가 연속적으로 사용되었어도 `배치(batch)` 처리에 의해서 한 번의 렌더링으로 최신 상태를 유지하는 것이다.

### 배치(batch)

> React가 너 나은 성능을 위해 여러 개의 state 업데이트를 하나의 리렌더링으로 묶는 것을 의미한다.
>
> React는 16ms 동안 변경된 상태 값들을 하나로 묶는다. (16ms 단위로 배치를 진행한다.)

이러한 문제를 해결하기 위해 해당 컴포넌트의 가장 최신(이전) 값을 사용하는 함수를 통해 전달 해주는 것이 좋다.

```jsx
const [numbers, setNumbers] = useState(0);

const Plus = () => {
  setNumbers((prev) => prev + 1);
  setNumbers((prev) => prev + 1);
  setNumbers((prev) => prev + 1);
};

return (
  <div>
    <p>{numbers}</p>
    <button onClick={Plus} />
  </div>
);
```

위처럼 작성하면,

1. 여러번 전달 받는 함수들이 `큐(queue)`에 저장되어 순서대로 실행하게 된다.
2. 큐에서 먼저 수행된 함수의 결과로 반영된 `state`값이
3. 다음에 있는 함수의 인자로 들어가게 되므로, 항상 최신의 `state`를 유지하게 된다.
