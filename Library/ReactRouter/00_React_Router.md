# React Router

**`React-Router-Dom` v6 정리**

```jsx

  import { createBrowserRouter, RouterPorvider } from 'react router-dom';

	const router = createBrowserRouter([
		{
			path: '/',
			element: <Root />
			errorElement: <p>Not Found</p> // 에러 경로
			children: [
				{ index: true, element: <Home /> },
				{ path: '/about', element: <About /> },
				{ path: '/about/:aboutId', element: <AboutDetail /> },
			]
		}
	]);

	function App() {
		return <RouterProvider router={router} />;
	}

	export default App;
```

# Router 종류

### HashRouter

URL의 해쉬(#)값을 이용하는 라우터

1. 주소에 해쉬(#)가 붙는다.
2. 검색 엔진이 읽지 못한다.
3. 별도의 서버 설정을 하지 않더라도 새로 고침 시 오류가 발생하지 않는다. 이는 `해시 라우터`가 해쉬(#) 뒤의 값은 브라우저에서만 관리하고 (라우팅 하는 사실을 서버가 모름) 서버는 기본 url로 서버에 데이터를 요청하기 때문이다.
4. `history API`를 사용하지 않기 때문에 동적 페이지에 불리하다.

### BrowserRouter

1. `history API` 를 사용한다.
2. 별도의 서버 설정이 없다면 새로고침 시 404에러가 발생한다.

   > 서버에서는 기본 라우트 `'/'`정보만 저장되어 있고, 이외의 모든 하위 라우팅은 이 default 경로를 통해 이루어지기 때문에 이 경로를 제외하고는 서버에서 인식하지 못한다. 만일 필요하다면 해당 주소와 그에 맞는 페이지를 서버에 알려주어야 한다. 그런게 아니라면 일반적으로는 `'/*'`의 경로로 접근 시 서버에는 `'/'` 로 리다이렉션 해주면 된다.
   >
   > (404 Error -> index.html)
   >
   > **배포할 때 주의**

3. 큰 프로젝트에 적합하다.

### `BrowserRouter`

- `history API`를 활용해 history 객체를 생성한다.
- `history API`는 내부적으로 `stack 자료구조`의 형태를 띄기 때문에 사용자가 방문한 url 기록들을 차곡차곡 쌓는다.
- 라우팅을 진행할 컴포넌트 상위에 `BrowserRouter` 컴포넌트를 생성하고 감싸준다.

### `Route`

- 현재 브라우저의 `location(window.href.location 정보를 가져온다)` 상태에 따라 다른 element를 렌더링 한다.
- `Route.element`: 조건이 맞을 때 렌더링할 element, `<Element />`의 형식으로 전달.
- `Route.path`: 현재 path값이 url과 일치하는지 확인해 해당 url에 매칭된 element를 렌더링 해준다.

### `Routes`

- 모든 `Route`의 상위 경로에 존재해야 하며, location 변경 시 하위에 있는 모든 `Route`를 조회해 현재 location과 맞는 `Route`를 찾아준다!

## Link

`Link 컴포넌트`는 라우터 내에서 직접적으로 페이지 이동을 하고자 할 때 사용되는 컴포넌트이다.

```jsx
import React from "react";
import { Link } from "react-router-dom";

function Nav() {
  return (
    <div>
      <Link to="/"> Home </Link>
      <Link to="/about"> About </Link>
    </div>
  );
}

export default Nav;
```

- 계층 구조에 상대적이다.
- 상대 경로 표현이 가능하므로, `./..` 과 같은 표현도 사용이 가능하다.
- `preventScrollReset`
  - 페이지 중간에 있는 컨텐츠 내부에서 tab 목록을 누르는 것과 같은 시도를 할 때, 기존의 Link 컴포넌트였다면 클릭 시 스크롤이 초기화되어 페이지 가장 위로 이동하게 된다.
  - 그러나 이 속성을 `true`로 설정해주면 이를 방지할 수 있다.
- `state`
  - `useLocation` 훅과 연계하여 특정 state를 넘겨주는 것도 가능하다.
    ```jsx
    let { state } = useLocation();

    <Link to="path" state={{ some: "value" }} />;
    ```

## **useNavigate**

`useNavigate`훅을 사용하면 특정 이벤트(`onChange`, `onClick` 등)가 발생했을 때 페이지 이동을 트리거할 수 있다.

```jsx
import { useNavigate } from "react-router-dom";

const navigate = useNavigate();

const onClick = () => {
  navigate("/");
};
```

- replace
  - 기본값은 `false`이고, `true` 로 설정한다면 이동 후 뒤로가기 가 불가능해진다.

```jsx
navigate("/", { replace: true });
```

- state
  - Link와 마찬가지로 state를 전달해줄 수 있다!!!

```jsx
navigate("/", { state: { id: id } });

const location = useLocation();
const { id } = location.state;
```

## **중첩 라우팅**

특정 페이지 내에서 하위 페이지를 만들 수 있고, 해당 페이지마다 경로를 이용한 데이터 전달 가능

또한 중첩 라우팅을 구현할 경우 해당 하위 페이지 이외에는 컨텐츠가 바뀌지 않는다.

```jsx
<Route path="/about" element={<About />}>
  <Route path="detail" element={<Detail />}></Route>
</Route>
```

라우터 내부에 위와 같이 자식 요소 `Route`를 만들어준다. 이렇게 설정하면 라우터 내부적으로는 `/about` 주소 하위에 `/detail` 이라는 하위 라우팅이 되었다고 판단한다. 따라서 우리가 `/about/detail`로 주소를 이동할 경우, 주어진 `Detail` 컴포넌트가 렌더링된다. (`About` 컴포넌트도 같이 렌더링 된다.)

물론 라우터에서 위와 같이 설정한 것 만으로는 아무런 변화가 생기지 않는다.

실제로 해당 라우팅이 발생하는 부모 요소인 About 페이지에 하위 라우팅 발생 시 컴포넌트를 렌더링할 자리를 명시해 주어야 하고, 이때 사용되는 것이 `Outlet` 컴포넌트이다.

### Outlet

```jsx
import { Outlet } from "react-router-dom";

function About() {
  return (
    <>
      <div>
        <h1>About</h1>
        <p>about page</p>
      </div>
      <Outlet />
    </>
  );
}
```

이처럼 About 컴포넌트 내부에 `Outlet` 컴포넌트를 렌더링 해주면 라우터에서 이를 인식하고 `Outlet` 자리에 Detail 컴포넌트를 렌더링하게 되는 것이다.(물론 주소가 일치하는 경우)

### Outlet 없이 중첩 라우팅 구현하기

Outlet 없이도 중첩 라우팅을 구현할 수 있다.

우선 라우터에서 중첩 라우팅을 하고자 하는 주소에 다음과 같이 `*` 을 추가해주어 중첩 라우팅이 발생할 주소임을 명시해준다.

```jsx
<Routes>
  <Route path="/" element={<Home />}></Route>
  <Route path="/about/*" element={<About />}></Route>
  <Route path="/products" element={<Products />}></Route>
</Routes>
```

이후 해당 About 컴포넌트에서 본래 `Outlet`이 들어갔던 자리에 마치 라우터를 구현했던 것처럼 중첩 라우팅을 진행해주면 된다.

```jsx
function About() {
  return (
    <div>
      <div>
        <h1>About</h1>
        <p>about page</p>
      </div>
      <Routes>
        <Route path="/location" element={<Detail />}></Route>
      </Routes>
    </div>
  );
}
```

위와 같이 작성하면 Outlet 과 동일한 기능을 하는 중첩 라우팅을 구현할 수 있다.

## Params

주소 경로 내부에 특정 데이터를 넣어 전달하는 것을 말하는데, 크게 url 파라미터와 쿼리스트링으로 나누어진다.

- `url 파라미터`

```jsx
<Route path="/new/:id" element={<NewId />} />
```

여기에서 `:`으로 구분해준 id라는 값은 파라미터로 전달되어 이후 우리가 컴포넌트 내부에서 `useParams` Hook을 통해 추출하고 사용할 수 있다.

- `쿼리스트링`

`?, &` 을 기준으로 key와 value를 나눠 데이터를 전달 받는다. 이렇게 전달 받은 값은 이후 `useLocation`훅을 통해 추출하고 사용할 수 있다.

이전까지는 `?, &` 을 직접 분리해 추출 해야 하는 번거로움이 있었는데, `useSearchParams` 를 사용하면 쉽게 해결할 수 있다.

| url 파라미터                             | 쿼리 스트링                                                                        |
| ---------------------------------------- | ---------------------------------------------------------------------------------- |
| ID, 이름, 특정 데이터를 조회할 때 사용   | 키워드 검색, 페이지네이션, 정렬 방식 등 데이터 조회에 필요한 옵션을 전달할 때 사용 |
| 일반적인 변수, 상수 값들을 전달하기 용이 | key, value 형태의 데이터이므로 json이나 객체 형태의 데이터를 전달하기 용이         |

### useParams

위에서 말한 것처럼 url 파라미터를 조회할 때 사용한다.

실제 사용 예시를 살펴보자!!

```jsx
import React from "react";
import { useParams } from "react-router-dom";

const NewId = () => {
  let { id } = useParams();

  return (
    <div className="test">
      <p>유저의 아이디는 {id} 입니다.</p>
    </div>
  );
};

export default NewId;
```

이처럼 http://test.com/new/1234 주소로 이동하여 렌더링된 NewId 컴포넌트 내부에서 useParams를 이용해 아이디 값을 받아올 수 있다.

http://test.com/new/1234/1111 와 같은 주소에 라우팅을 `/new/:id/lastId` 와 같은 방식으로 해준다면 여러 개의 값도 한 번에 전달 받을 수 있다.

### useSearchParams

위에서 언급한 것처럼 쿼리스트링을 추출하는 데 사용된다. 현재 위치에 대한 url의 쿼리스트링을 읽고 수정할 때 사용한다!!

```jsx
const [serchParams, setSearchParams] = useSearchParams();
```

`searchParams`는 `URLSearchParams` 객체이면서 `쿼리스트링`을 다루기 위한 다양한 메서드를 내장하고 있다. 함수의 인자에 객체와 문자열을 넣어주면 현재 url의 쿼리스트링을 변경하는 기능을 제공한다.

### 값을 읽어오는 메서드

`searchParams.get(key)` : 특정한 key의 value를 가져오는 메서드, 해당 key의 value가 두 개 이상이라면 처음의 값을 반환한다.

`searchParams.getAll(key)` : 특정 key에 해당하는 모든 value를 가져오는 메서드.

### 값을 변경하는 메서드

`searchParams.set(key, value)` : 인자로 전달한 key 값을 value로 설정한다. 만일 기존에 key에 대한 값이 존재했다면 덮어씌운다.

`searchParams.append(key, value)` : 기존 값을 변경 혹은 삭제하지 않고 추가한다.

> `searchParams` 를 변경하는 메서드로 값을 변경하더라도 실제 url 에는 이 정보가 반영되지 않는다. 만일 이를 반영하고자 한다면 `setSearchParams`에 `searchParams`를 인자로 전달해주어야 한다.

> 쿼리스트링은 페이지네이션이나 키워드 검색, 정렬 등 꽤 다양한 곳에서 용이하게 사용된다.
