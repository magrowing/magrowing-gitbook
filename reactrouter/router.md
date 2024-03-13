# React Router

## 학습 키워드

- Router
- React Router
  - BrowserRouter
    - history API
  - Routes
  - Route
  - MemoryRouter
- 동적 라우팅
  - URL Parameter
  - useParams
  - Query String
  - useSearchParams

<br/>

## [Router](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%84%B0)

- 컴퓨터 네트워크 간에 데이터 패킷을 전송하는 네트워크 장치다.
- 즉, 서로 다른 네트워크 간에 최적의 경로를 찾아내는 알고리즘을 활용해 __중계 역할을 해주는 장치다.__

![Routing Diagram](./image/routing_diagram.svg)

<br/>

## React Router

- React에서 주소(URL)에 따른 컴포넌트 렌더링을 관리하기 위한 라이브러리
- Context API를 기반으로 한 라이브러리

### 🤔 왜 React Router 을 사용해야 하는가?

웹 애플리케이션에서 라우팅이라는 개념은 사용자가 요청한 URL에 따라 알맞는 페이지를 보여주는 것을 의미한다. 웹 애플리케이션을 만들때 프로젝트를 하나의 페이지로 구성할 수도 있고, 여러 페이지를 구성할 수도 있다.
예를 들어 블로그를 만든다면, 홈, 포스트 목록, 포스트, 글쓰기 등의 다양한 페이지들이 있다. 또한 이 페이지들에 따라 주소(URL)도 만들어줘야 한다. 주소가 있어야, 유저들이 북마크도 할 수 있고 서비스에 구글을 통해 유입될 수 있기 때문이다. 이렇게 여러 페이지로 구성된 웹 애플리케이션을 만들 때 페이지별로 컴포넌트들을 분리해가면서 프로젝트를 관리하기 위해 필요한 것이 바로 라우팅 시스템(React Router)이다.

<br/>

### 🤖 React Router 의 역활

사용자의 브라우저 주소창의 경로에 따라 알맞는 페이지를 보여준다. 이후 링크를 눌러서 다른 페이지로 이동하게 될 때 서버에 다른 페이지의 html을 새로 요청하는 것이 아니라, __브라우저의 History API를 사용하여 브라우저의 주소창의 값만 변경하고__ 기존에 페이지에 띄웠던 웹 애플리케이션을 그대로 유지하면서 라우팅 설정에 따라 또 다른 페이지를 보여주게 된다.

<br/>

## React Router 사용법

### ⚙️ React Router 설치

```shell
npm i react-router-dom
```

<br/>

### [BrowserRouter](https://reactrouter.com/en/main/router-components/browser-router)

- HTML5의 History API를 사용하여 페이지를 새로 불러오지 않고도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 해 주는 역활을 수행한다.
- 라우팅을 진행할 컴포넌트 상위에 `BrowserRouter` 컴포넌트를 생성하고 감싸주어야 한다.

#### 📖 [History API](https://developer.mozilla.org/ko/docs/Web/API/History_API)

> history 전역 객체를 통해 브라우저 세션 히스토리에 대한 접근을 제공합니다.

브라우저에서 페이지 로딩을 하면, 세션 히스토리를 갖는다. 브라우저는 이 히스토리를 stack으로 관리한다.
세션 히스토리는 페이지를 이동할 때 마다 쌓이며, 이를 통해 `뒤로가기` 또는 `앞으로 가기` 같은 이동이 가능하다.

__⇒ "어떤 페이지를 탐색했는지에 대해서 history를 쌓는 것" 이라고 생각하면 된다.__

#### 🤔 왜 History API를 사용하게 되는가?

- React는 SPA의 기반이다. 페이지가 로딩이 되어 변경되는것 처럼 보이지만 컴포넌트를 통해 화면을 업데이트(리렌더링)해준다.
그렇기 때문에 실제로는 화면 이동이 일어나지 않는다. 그렇기에 History API를 사용하게 되면 화면 이동 없이 URL을 업데이트 해줄 수 있다.

<br/>

### Routes & Route

- Routes와 Route는 React Router v6부터 도입된 개념이다.

### [Routes](https://reactrouter.com/en/main/components/routes)

- 모든 `Route의 상위 경로`에 존재해야 하며, location 변경 시 하위에 있는 모든 Route를 조회해 현재 location과 맞는 Route를 찾아준다.

### [Route](https://reactrouter.com/en/main/route/route)

- 현재 브라우저의 location(window.location.path 정보를 가져온다)상태에 따라 다른 element를 렌더링한다.

```jsx
<Route path="/경로" element={<Component />} />
```

<br/>

### [MemoryRouter](https://reactrouter.com/en/main/router-components/memory-router)

- 브라우저 환경이 아닌 곳에서 ReactRouter가 포함된 컴포넌트를 테스트 할 때 사용한다.
- `MemoryRouter`는 주소를 가지고 있지 않아, 주소를 따로 잡아주어야 한다.

```jsx
<MemoryRouter initialEntries={['/경로']} >
```

<br/>

#### 👩🏻‍💻 React Router 간단한 예제

```jsx
// main.jsx

import React from 'react';
import ReactDOM from 'react-dom/client';

import { BrowserRouter } from 'react-router-dom';

import App from './App';

function main() {
  const container = document.getElementById('root');
  if (!container) {
    return;
  }

  const root = ReactDOM.createRoot(container);
  root.render(
    <React.StrictMode>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </React.StrictMode>
  );
}

main();
```

```jsx
// App.tsx

import { Routes, Route } from 'react-router-dom';

import Homepage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

import Header from './components/Header';
import Footer from './components/Footer';

export default function App() {
  return (
    <div>
      <Header />
      <main>
        <Routes>
          <Route path="/" element={<Homepage />} />
          <Route path="/about" element={<AboutPage />} />
        </Routes>
      </main>
      <Footer />
    </div>
  );
}
```

```jsx
// App.test.tsx

import { render, screen } from '@testing-library/react';

import { MemoryRouter } from 'react-router-dom';

import App from './App';


describe('App', () => {

  function renderApp(path: string) {
    render((
    <MemoryRouter initialEntries={[path]}> 
      <App />
    </MemoryRouter>
    ));
  }
 
  context('when the current path is “/”', () => {
    it('renders the home page', () => {
    renderApp('/');

    screen.getByText(/Hello/);
    });
  });
 
  context('when the current path is “/about”', () => {
    it('renders the about page', () => {
    renderApp('/about');

    screen.getByText(/About/);
    });
  });
});
```

<br/>

## 동적 라우팅

- 동적 Routing은 __경로를 미리 정해두지 않고 동적으로 설정하는 방식이다.__
- 라우트의 경로에 특정 값을 넣어 해당 페이지로 이동 할 수 있게 하는 것

#### 🤔 동적 라우팅이 필요한 이유는?

React Router 를 사용해서 미리 프로젝트에 사용할 경로들과 보여줄 컴포넌트를 정의해둔다.
하지만 복잡하고 규모가 큰 애플리케이션에서는 경로를 미리 설정하는 방식만으로는 처리하기 힘든 작업이 존재한다.

예를 들어 쇼핑몰에는 다양한 상품이 존재하고, 상품리스트가 있고, 또한 상품 상세페이지도 존재한다. 그렇다면 상품리스트에서 선택한 상품의 상세 페이지에 접근 하기 위해서는 어떻게 해야 할까?

> 💡 특정 규칙을 만들고, 그 규칙과 부합하는 URL이 있을 경우에만 해당 element를 화면에 보여주는 방식으로 해결한다.

<br/>

### URL Parameter

- `/:값` 형태로 동적인 데이터를 전달한다.
- `:` 기호 뒤에 붙는 문자열이 Path Parameter 이다.
- Path Parameter 는 URL에 있는 값을 마치 매개변수(Parameter)처럼 사용하는 것이다.
- Path parameter를 이용하면 사용자가 같은 페이지로 접속하더라도, 큰 틀은 동일하되 다른 UI를 보여주도록 처리할 수 있다.

```jsx
function App() {

  return (
    <Layout>
      <Routes>
        <Route path={'/'} element={<Home/>}></Route>
        <Route path={'/search'} element={<Search/>}></Route>
        <Route path={'/country/:code'} element={<Country/>}></Route>
        <Route path={'*'} element={<NotFound/>}></Route>
      </Routes>
    </Layout>
  );
}
export default App
```

### ⚙️ [useParams](https://reactrouter.com/en/main/hooks/use-params)

- URL Params의 값을 객체 형태로 반환한다.
- `key` : Route 에서 설정한 Path Parameter의 이름
- `value` : Route에서 설정한 Path Parameter에 실제로 전달된 값

> /post/:id로 path를 설정했을 때, 유저가 /post/1로 접속할 경우 useParams가 반환하는 객체의 key는 id이고, value는 1이다.

```jsx
import { useParams } from 'react-router-dom';

export default function Country(){
   const params = useParams();
   console.log(params); // {code : 'KOR'}
   return(
      <div>
         Country! {params.code}
      </div>
   );
}
```

<br/>

### QueryString

- URL에 쿼리 스트링을 포함시켜주면 된다. 특별한 설정을 할 필요는 없다.
  - Link 컴포넌트 예시 : `<Link to="/list?sort=popular" />`
  - navigate 함수 예시 : `navigate("/list?sort=popular")`

### ⚙️ [useSearchParams](https://reactrouter.com/en/main/hooks/use-search-params)

- QueryString (예 : ?sort=popular&sort=latest) 에서 원하는 값만 꺼내올 수 있도록 도와주는 Hook

```jsx
const [searchParams, setSearchParams] = useSearchParams();
```

- searchParams.get(key) : 쿼리 스트링에서 특정 key의 value 값 반환 (하나만)
- searchParams.getAll(key) : 쿼리 스트링에서 특정 key의 모든 value 값을 배열로 반환
- searchParams.toString() : 객체 형태의 쿼리 스트링을 문자열 형태로 반환

<br/>

## 🔗 참고

- [React Router에 대해 알아보자](https://velog.io/@jeong_lululala/react-router-routes)
- [React-Router-Dom 개념잡기](https://velog.io/@kandy1002/React-Router-Dom-개념잡기)
- [React Router React Router v6 튜토리얼](https://velog.io/@velopert/react-router-v6-tutorial)
- [History API](https://velog.io/@minw0_o/history-API란)
- [프로젝트로 배우는 React.js & Next.js 마스터리 클래스](https://www.udemy.com/share/109oZ43@XS9FkiC8txXm2etyCS6hUlW6ZOVqD2qk_sJ7LrK8tdykLM4e8LanybWgL0RA8r-GWA==/)
- [Dynamic Routing & Query String](https://sylagape1231.tistory.com/112)
