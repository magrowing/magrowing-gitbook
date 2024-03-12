# React Router

## 학습 키워드

- Router
- React Router
  - BrowserRouter
    - history API
  - Routes
  - Route
  - MemoryRouter
  - URL 파라미터 & 쿼리스트링

<br/>

## [Router](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%84%B0)

- 컴퓨터 네트워크 간에 데이터 패킷을 전송하는 네트워크 장치다.
- 즉, 서로 다른 네트워크 간에 최적의 경로를 찾아내는 알고리즘을 활용해 __중계 역할을 해주는 장치다.__

![Routing Diagram](./image/routing_diagram.svg)

<br/>

## React Router

- React에서 주소(URL)에 따른 컴포넌트 렌더링을 관리하기 위한 라이브러리
- Context API를 기반으로 한 라이브러리

### 🤔 왜 React Router을 사용해야 하는가?

블로그를 만든다면, 홈, 포스트 목록, 포스트, 글쓰기 등의 다양한 페이지들이 있다. 또한 이 페이지들에 따라 주소(URL)도 만들어줘야 한다. 주소가 있어야, 유저들이 북마크도 할 수 있고 서비스에 구글을 통해 유입될 수 있기 때문이다.

다른 주소에 따라 다른 뷰를 보여주는것을 Routing 이라고 하는데, 리액트 자체에는 이 기능이 내장 되어있지 않기 때문이다.

따라서 우리가 직접 브라우저의 API 를 사용해서 상태를 설정하여 다른 뷰를 보여주어야 한다.

<br/>

### 🤖 React Router의 역활

사용자의 브라우저 주소창의 경로에 따라 알맞는 페이지를 보여준다. 이후 링크를 눌러서 다른 페이지로 이동하게 될 때 서버에 다른 페이지의 html을 새로 요청하는 것이 아니라, `브라우저의 History API를` 사용하여 __브라우저의 주소창의 값만 변경하고__ 기존에 페이지에 띄웠던 웹 애플리케이션을 그대로 유지하면서 라우팅 설정에 따라 또 다른 페이지를 보여주게 된다.

<br/>

## React Router 사용법

### ⚙️ React Router 설치

```shell
npm i react-router-dom
```

<br/>

### [BrowserRouter](https://reactrouter.com/en/main/router-components/browser-router)

- HTML5의 History API를 사용하여 페이지를 새로 불러오지 않고도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 해 주는 역활을 수행한다.
- 라우팅을 진행할 컴포넌트 상위에 BrowserRouter 컴포넌트를 생성하고 감싸주어야 한다.

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

- 모든 Route의 상위 경로에 존재해야 하며, location 변경 시 하위에 있는 모든 Route를 조회해 현재 location과 맞는 Route를 찾아준다.

### [Route](https://reactrouter.com/en/main/route/route)

- 현재 브라우저의 location(window.href.location 정보를 가져온다)상태에 따라 다른 element를 렌더링한다.

```jsx
<Route path="/경로" element={<Component />} />
```

<br/>

### [MemoryRouter](https://reactrouter.com/en/main/router-components/memory-router)

- 브라우저 환경이 아닌 곳에서 ReactRouter가 포함된 컴포넌트를 테스트 할 때 사용한다.
- `MemoryRouter`는 주소를 가지고 있지 않아, 주소를 따로 잡아주어야 한다.

<br/>

<br/>

## 🔗 참고

- [React Router에 대해 알아보자](https://velog.io/@jeong_lululala/react-router-routes)
- [React-Router-Dom 개념잡기](https://velog.io/@kandy1002/React-Router-Dom-개념잡기)
- [React Router React Router v6 튜토리얼](https://velog.io/@velopert/react-router-v6-tutorial)
- [History API](https://velog.io/@minw0_o/history-API란)
