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
