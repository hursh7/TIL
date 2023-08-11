# 20230811 (Promise API)

# Promise.all

- 요소 전체가 프로미스인 배열(엄밀히 따지면 이터러블 객체이지만, 대게는 배열)을 받고 새로운 프로미스를 반환한다.
  배열 안 프로미스가 모두 처리되면 새로운 프로미스가 이행되고, 배열 안 프로미스의 결과 값을 담은 배열이 새로운 프로미스의 `result`가 된다.

      ```jsx
      Promise.all([
        new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
        new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
        new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
      ]).then(alert);
      // 3초 후에 처리되고, 반환되는 프라미스의 result는 배열 [1, 2, 3]이 된다.
      ```

- 배열 `result`의 요소 순서는 `Promise.all`에 전달되는 프로미스 순서와 상응한다. `Promise.all`의 첫 번째 프로미스는 가장 늦게 이행되더라도 처리 결과는 배열의 첫 번째 요소에 저장된다.
- **`Promise.all`에 전달되는 프로미스 중 하나라도 거부되면, `Promise.all`이 반환하는 프로미스는 에러와 함께 바로 거부된다.**

# Promise.allSettled

- `Promise.all`은 프로미스가 하나라도 거절되면 전체를 거절한다. 반면, `Promise.allSettled` 는 모든 프로미스가 처리될 때 까지 기다린다. 반환되는 배열은 다음과 같은 요소를 가진다.
  - 응답이 성공할 경우: `{status: "fulfilled", value: result}`
  - 에러가 발생한 경우: `{status: "rejected", reason: error}`

```jsx
let urls = [
  'https://api.github.com/users/success1',
  'https://api.github.com/users/success2',
  'https://error'
];

Promise.allSettled(urls.map(url => fetch(url)))
  .then(results => {
    results.forEach((result, num) => {
      if (result.status == "fulfilled") {
        alert(`${urls[num]}: ${result.value.status}`);
      }
      if (result.status == "rejected") {
        alert(`${urls[num]}: ${result.reason}`);
      }
    });
  });

// results ...
  [
    {status: 'fulfilled', value: ...응답...},
    {status: 'fulfilled', value: ...응답...},
    {status: 'rejected', reason: ...에러 객체...}
  ]
```

💡 스펙에 추가된 지 얼마 안 된 문법이라, 구식 브라우저는 폴리필이 필요하다.

# Promise.race

- 마찬가지로 프로미스로 구성된 배열을 인자로 받지만 all 과는 달리 모든 요청이 완료되기까지 기다리지 않고 `가장 먼저 성공적으로 이행되거나 실패한 프로미스`를 반환한다.
  ```jsx
  Promise.race([
    new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
    new Promise((resolve, reject) =>
      setTimeout(() => reject(new Error("에러 발생!")), 2000)
    ),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
  ]).then(alert); // 1
  ```
- 첫 번째 프라미스가 가장 빨리 처리 상태가 되기 때문에 첫 번째 프로미스의 결과가 `result` 값이 된다. 이렇게 `Promise.race` 를 사용하면 '경주(race)의 승자’가 나타난 순간 다른 프로미스의 결과 또는 에러는 무시된다.
- 서버로부터 일정 시간 응답이 없을 경우 수동으로 오류 처리를 하기 위해 많이 사용된다.

> 💡 `Promise.all` 의 경우 이미지를 업로드하는 작업같이 `순서가 보장되지 않아도 상관없는 비동기 태스크를 병렬적으로 처리` 할 때 유용하게 사용 할 수 있고, `Promise.race`는 로딩 시간이 너무 짧거나 길 때 `최소 로딩 시간과 최대 로딩 시간을 설정` 할 때 유용하게 사용할 수 있다.
