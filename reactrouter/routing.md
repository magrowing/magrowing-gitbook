# Routing

## 학습 키워드

- HTML DOM API
  - Location
  - pathname
  - hash
- Routing

<br/>

## [HTML DOM API](https://developer.mozilla.org/ko/docs/Web/API/HTML_DOM_API)

> HTML의 각 elements의 기능을 정의하는 인터페이스와 해당 요소가 의존하는 모든 지원 유형 및 인터페이스로 구성됩니다.

#### HTML DOM API에 포함 된 기능

- DOM을 통한 HTML 요소에 대한 접근 및 제어.
- 양식 데이터에 대한 접근 및 조작.
- 2D 이미지의 콘텐츠 및 HTML `canvas`의 맥락과 해당 요소 위에 그리는 것과 같은 상호 작용.
- HTML 미디어 요소 (`audio`및 `video`)에 연결된 미디어 관리.
- 웹 페이지에서 콘텐츠 드래그 앤 드롭.
- 브라우저 탐색 기록에 대한 접근
  - Web Components, Web Storage, Web Workers, WebSocket 및 Server-sent events와 같은 기타 API에 대한 연결 인터페이스 지원.

### [Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

> Location 인터페이스는 객체가 연결된 장소(URL)를 표현합니다. Location 인터페이스에 변경을 가하면 연결된 객체에도 반영되는데, Document와 Window 인터페이스가 이런 Location을 가지고 있습니다. 각각 Document.location과 Window.location으로 접근할 수 있습니다.

- Location은 즉 URL의 정보를 담고 있다는걸 의미

#### 🤔 [Window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)은 그럼 무엇일까?

> 읽기 전용 속성으로, 문서의 현재 위치에 대한 정보가 담긴 Location 객체를 반환합니다.

- 브라우저내에서 현재 페이지의 위치 URL의 정보에 대해 알 수 있는 속성

<br/>

### [PathName](https://developer.mozilla.org/ko/docs/Web/API/URL/pathname)

> URL 인터페이스의 pathname 속성은 URL의 경로와 그 앞의 `/`로 이루어진 USVString을 반환한다.

- URL의 구성요소 path를 의미

![URL](../network/image/url.png)

```javascript
var url = new URL(
  "https://developer.mozilla.org/ko/docs/Web/API/URL/pathname?q=value",
);
var result = url.pathname; // "/ko/docs/Web/API/URL/pathname"
```

<br/>

### [hash](https://developer.mozilla.org/en-US/docs/Web/API/Location/hash)

> The hash property of the Location interface returns a string containing a '#' followed by the fragment identifier of the URL — the ID on the page that the URL is trying to target.

- URL 내의 `#` 뒤에 나오는 식별자를 value로 하는 DOMstring

#### hash의 쓰임새

- #id를 활용해 클릭 할 때 지정한 Anchor로 이동  
- <https://developer.mozilla.org/ko/docs/Web/API/Location#예제>

```
encodeURI('예제')

'#%EC%98%88%EC%A0%9C'
```

```
decodeURI(location.hash)

'#예제'
```

<br/>

## [Routing](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%8C%85)

> 어떤 네트워크 안에서 통신 데이터를 보낼 때 최적의 경로를 선택하는 과정이다.

- 라우터가 수신한 패킷을 최적의 경로로 전달하기 위한 과정
- 데이터를 목적지까지 전달하기 위한 모든 일련의 과정

![Routing Diagram](./image/routing_diagram.svg)

#### 🤔 예시를 통해 이해보자면

1. 우리가 서울에서 부산을 간다고 했을때 버스를 타고 갈 수도 있고, 비행기를 탈 수도 있고, 기차를 탈 수도 있다.
→ 경로들 중에서 하나를 선택하는 것이 라우팅이라 할 수있다.

2. 지도앱을 사용해서 경로를 검색할때 여러가지 경로가 나오기 전에 로딩시간이 있는데 이러한 로딩시간을 라우팅이라고 할 수도 있다.

<br/>

### 라우팅 구현하기

일반적인 웹 사이트는 __URL에 따라 다른 웹 페이지를 보여준다.__<br/>
React에서는 하나의 웹 페이지를 하나의 컴포넌트로 만들고, URL에 따라 적절한 컴포넌트가 보이게 함으로써 구현한다.

#### Example 1

```jsx
import Header from './components/Header';
import Footer from './components/Footer';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

function App() {
 const { pathname } = window.location; // url에서 pathname 추출 하는 속성
 
 return (
  <div>
   <Header />
   <main>
    {pathname === '/' && <HomePage />} 
    {pathname === '/about' && <AboutPage />}
   </main>
   <Footer />
  </div>
 );
}
```

#### Example 2

```jsx
import Header from './components/Header';
import Footer from './components/Footer';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

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

## 🔗 참고

- [Location.hash 로 URL을 사용하는 목적](https://webroadcast.tistory.com/1)
- [라우팅이란? 무엇인가?](https://dentuniverse.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EB%9D%BC%EC%9A%B0%ED%8C%85%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EC%9A%B0%EC%A3%BC%EB%A5%BC%EB%86%80%EB%9D%BC%EA%B2%8C%ED%95%98%EC%9E%90)
- [라우팅이란?](https://annajin.tistory.com/71)
- [Routing](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week7/routing#id-1.-routing)
