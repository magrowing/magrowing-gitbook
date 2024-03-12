# React Router

## 학습 키워드

- 라우터란?
- React Router
  - Browser Router
  - Routes & Route
  - Memory Router
  - RouterProvider

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

> 그래서 `React Routing 구현하기` 위해 window.location.pathname 사용해서 Routing을 구현했다.

<br/>

### 🤖 React Router의 동작원리

사용자의 브라우저 주소창의 경로에 따라 알맞는 페이지를 보여준다. 이후 링크를 눌러서 다른 페이지로 이동하게 될 때 서버에 다른 페이지의 html을 새로 요청하는 것이 아니라, `브라우저의 History API를` 사용하여 __브라우저의 주소창의 값만 변경하고__ 기존에 페이지에 띄웠던 웹 애플리케이션을 그대로 유지하면서 라우팅 설정에 따라 또 다른 페이지를 보여주게 된다.

<br/>

## React Router 사용법

### ⚙️ React Router 설치

```shell
npm i react-router-dom
```

### Router 의 종류

- Browser Router
  - HTML5의 History API를 사용하여 페이지를 새로 불러오지 않고도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 해 주는 역활을 수행한다.

- HasRouter

### Routes & Route

- Routes와 Route는 React Router v6부터 도입된 개념이다.
- `Routes`는 여려개의 Route 컴포넌트를 그룹화하고 관리하기 위한 컴포넌트이다.
- `Route`는 실제로 주어진 경로에 대한 컴포넌트 렌더링 하기 위한 컴포넌트이다.
