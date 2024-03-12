# React Router Upgrading

## 학습 키워드

- React Router 6.4
  - createBrowserRouter
  - createMemoryRouter
  - RouterProvider
  - Outlet

<br/>

## React Router 6.4

React Router 버전 6.4부터 지원하는 `라우터 객체`를 만들어서 사용해보자.

 > 아래와 같은 방식으로 React Router를 이용해서 라우터 객체를 만들어서 사용해보자.

```jsx
import Header from './components/Header';
import Footer from './components/Footer';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

// ⬇️ React Router를 이용해서 라우터 객체를 만들어 보자!
const pages = {
  '/': HomePage,
  '/about': AboutPage,
};

export default function App() {
  const path = window.location.pathname;

  const Page = Reflect.get(pages, path) || HomePage;

  return (
    <div>
      <Header />
      <main>
        <Page />
      </main>
      <Footer />
    </div>
  );
}
```

<br/>

## React Router v6.4 의 사용법

### 🤔 App 컴포넌트의 2가지 역활

현재 App 컴포넌트는 2가지 역할을 하고 있다. `어떤 레이아웃을 취하고 있는지` 와 `어떻게 라우팅이 되는지` 이다.

```jsx
// App.tsx 

import { Routes, Route } from 'react-router-dom';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

import Header from './components/Header';
import Footer from './components/Footer';

export default function App() {
  return (
    <div>
      <Header />
      <main>
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/about" element={<AboutPage />} />
        </Routes>
      </main>
      <Footer />
    </div>
  );
}
```

### 📑 App 컴포넌트에서 2가지 역활을 분리해보자

RoutingPage 컴포넌트를 생성해서 라운팅과 관련된건 넘겨주고, App컴포넌트는 레이아웃 관리하도록 분리되었다.
그렇다면 RoutingPage 를 객체방식으로 만들어도 되지 않을까?

```jsx
import { Routes, Route, createBrowserRouter } from 'react-router-dom';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

import Header from './components/Header';
import Footer from './components/Footer';

function RoutingPage() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
      <Route path="/about" element={<AboutPage />} />
    </Routes>
  );
}

export default function App() {
  return (
    <div>
      <Header />
      <main>
        <RoutingPage />
      </main>
      <Footer />
    </div>
  );
}
```

### 📌 라우팅 처리를 독립시키자

Route 컴포넌트의 path와 element를 맵핑한 객체를 가지도록 한다.

```jsx
const routes = [
  {path: '/', element: <HomePage />},
  {path: '/about', element: <AboutPage />},
];
```

[createBrowserRouter](https://reactrouter.com/en/main/routers/create-browser-router)를 이용해서 라우터 객체를 생성한다. 이렇게 생성된 라우터 객체는 라우팅을 처리하는데 사용한다.

```jsx
const router = createBrowserRouter(routes);
```

그리고 [RouterProvider](https://reactrouter.com/en/main/routers/router-provider)를 이용해서 router 객체를 쓰겠다고 한다. 이렇게 하면 애플리케이션 전체에서 라우팅을 처리할 수 있게 된다.

```jsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

const routes = [
  { path: '/', element: <HomePage /> },
  { path: '/about', element: <AboutPage /> },
];

const router = createBrowserRouter(routes);


export default function App() {
  return (
    <RouterProvider router={router}></RouterProvider>
  )
}
```

> 더 이상 BrowserRouter가 라우팅을 관리하지 않는다. createBrowserRouter 함수와 RouterProvider 컴포넌트를 사용하여 라우팅을 설정하고 관리한다. 때문에 main 컴포넌트에서 BrowserRouter 컴포넌트는 제거한다.

### 📌 레이아웃 독립 시키기

분기시점이 없기 때문에 위의 방식처럼 사용하는건 너무 불편하다.
레이아웃 자체를 다음과 같이 컴포넌트로 독립시킨다.

```jsx
function Layout() {
  return (
    <div>
      <Header />
      <main>
        {/* <RoutingPage /> */}
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}
```

routes한테 Layout 컴포넌트에 적용한다는 것을 잡아줘야 한다. 그려지는 것은 모두 `<Layout />`으로 그려지도록 해야 한다.

이를 위해서 routes를 계층형으로 바꾸었다.

```jsx
const routes = [
  {
    element: <Layout />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/about', element: <AboutPage /> },
    ],
  },
];
```

다만 Layout 컴포넌트가 HomePage 컴포넌트와 AboutPage 컴포넌트를 인지해서 올바른 위치에 넣어야 한다. 이는 React Router가 지원하는 컴포넌트인 Outlet 을 쓰면된다.

Outlet 컴포넌트는 Layout 컴포넌트 안에 정의된 레이아웃 구조를 그대로 유지하면서 컴포넌트를 동적으로 렌더링 할 수 있다.

```jsx
import { createBrowserRouter, RouterProvider, Outlet } from 'react-router-dom';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

import Header from './components/Header';
import Footer from './components/Footer';

function Layout() {
  return (
    <div>
      <Header />
      <main>
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}

const routes = [
  {
    element: <Layout />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/about', element: <AboutPage /> },
    ],
  },
];

const router = createBrowserRouter(routes);

export default function App() {
  return <RouterProvider router={router}></RouterProvider>;
}
```

### 📁 파일로 분리하자

routes는 테스트 때 필요한 정보이다. 그래서 App.tsx 파일에서 분리하는게 가져다쓰기에 좋다.

```
- src
  - components
    - Footer.tsx
    - Header.tsx
    - Layout.tsx ✅
 - pages
   - AboutPage.tsx
   - HomePage.tsx
- App.tsx
- main.tsx
- routes.tsx ✅
```

### 🧐 App 컴포넌트는 할 일이 없다?

App 컴포넌트가 하는 일이 줄었다. 이 일은 main 컴포넌트에 보내도 되는 일이라서 App 컴포넌트가 필요 없어졌다.

```jsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

import routes from './routes';

const router = createBrowserRouter(routes);

export default function App() {
  return <RouterProvider router={router}></RouterProvider>;
}
```

### 🛠️ 라우팅 테스트 하기

이전에는 App.test.tsx로 App 컴포넌트를 테스트했다.
이제는 routes가 컴포넌트와 레이아웃에 대한 정보를 모두 들고 있다. 때문에 routes만 테스트 하면 된다.

`MemoryRouter`는 주소를 가지고 있지 않아, 주소를 따로 잡아 준 것처럼 [createMemoryRouter](https://reactrouter.com/en/main/routers/create-memory-router)를 사용해서 테스트 한다.

```jsx
// App.test.tsx → routes.test.tsx 

import { render, screen } from '@testing-library/react';

import { createMemoryRouter, RouterProvider } from 'react-router-dom';

import routes from './routes';

const context = describe;
describe('App', () => {
  
  function renderRouter(path: string) {
    const router = createMemoryRouter(routes, { initialEntries: [path] });
    render(<RouterProvider router={router} />);
  }

  context('when the current path is “/”', () => {
    it('renders the home page', () => {
      renderRouter('/');

      screen.getByText(/환영/);
    });
  });
  
  context('when the current path is “/about”', () => {
    it('renders the about page', () => {
      renderRouter('/about');

      screen.getByText('about에 대한 정보를 가지고 있어요!');
    });
  });
});
```

<br/>

## 🔗 참고

- [React Router 버전 6.4부터 지원하는, 라우터 객체를 만들어서 쓰는 방법에 대해 알아보자](https://velog.io/@jeong_lululala/react-router-router)
