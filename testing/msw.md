# MSW

## 학습 키워드

- Service Worker
- MSW(Mock Service Worker)
- polyfill(폴리필)

<br/>

## Service Worker

### 📖 [Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)

- 웹 응용 프로그램, 브라우저, 그리고 (사용 가능한 경우) 네트워크 사이의 __프록시 서버 역활 수행__
- 서비스 워커는 출처와 경로에 대해 등록하는 이벤트 기반 워커로서 JavaScript로 작성 된 파일

서비스 워커는 연관된 웹 페이지/사이트를 통제하여 탐색과 리소스 요청을 가로채 수정하고, 리소스를 굉장히 세부적으로 캐싱힌다.

__⇒ 웹 앱이 어떤 상황에서 어떻게 동작해야 하는지 완벽하게 바꿀 수 있다. (대표적인 상황은 네트워크를 사용하지 못하는 경우)__

<br/>

## MSW(Mock Service Worker)

### 🌎 MSW 사용 배경(feat.Mocking)

프론트엔드 개발을 진행하다보면 백엔드 개발과의 종속적인 부분이 생기기 마련이다. (대표적으로 백엔드의 API를 활용해야 하는 경우)

> 기획자 : XX 작업은 어떻게 진행 중인가요? <br/>
프론트엔드 개발자 : 그게… 아직 API가 준비되지 않아서 다음 주까지는 기다려야 합니다.....

예상한 기간보다 API 개발에 시간이 더 필요해진 경우, 그 시간만큼
프론트엔드 개발자가 개발을 진행하지 못하는 상황이 생겨나기도 한다.

계속 기다릴 수는 없기에 __Mocking을 통해 위의 문제를 해결하고자 했다.__

[Thinking in React 예제를 통해 API 요청 코드 모킹](https://magrowing.gitbook.io/magrowing-gitbook/category/testing/react_testing_library#mocking)처럼 직접적으로 내부 로직에 직접 Mocking해서 필요한 화면에 붙이는 방식이 있지만,

- 서비스 로직에 직접 Mocking을 해야 하므로 애플리케이션 서비스 로직을 수정 필요
- HTTP 메소드와 네트워크의 응답 상태에 따라 각각 대응하기가 쉽지 않다.
- (Mocking으로 만든 결과물)화면에 대한 테스팅 및 디버깅 시에 어려움이 발생 .....

💡 결국 실제 API를 사용하는 것처럼 네트워크 수준에서 Mocking 하길 원하기 때문에 __네트워크 요청 과정에서 Request에 대한 Mocking이 가능한 MSW를 사용하는 것이다.__

<br/>

### 📖 [MSW](https://v1.mswjs.io/)는 무엇인가?

- Mock Service Worker의 약자
- API Mocking 라이브러리
- 네트워크 요청을 가로채서 모의 응답(Mocked response)을 보내주는 역할을 수행

<br/>

### ⚙️ MSW 설치 및 설정

- [Set up Mock Service Worker in Node.js](https://mswjs.io/docs/integrations/node)

#### 1. MSW 패키지 설치

```shell
npm i -D msw@0.36.4 
```

#### 2. `jest.config.js` 파일의 “setupFilesAfterEnv” 속성에 setupTests.ts 파일 추가

```js
// jest.config.js

module.exports = {
 testEnvironment: 'jsdom',
 setupFilesAfterEnv: [
  '@testing-library/jest-dom/extend-expect',
  '<rootDir>/src/setupTests.ts', //👈🏻 이부분추가
 ],
```

#### 3. `setupTests.ts` 파일 생성

```shell
  touch src/setupTests.ts
```

```ts
// src/setupTests.ts

import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```

- beforeAll : Jest 시작할 때 맨 처음에 실행
  - server.listen : 테스트가 실행되기 전 모킹 활성화
  - `onUnhandledRequest: 'error'` : handler를 안 잡았을 때 오류 내도록 설정
- afterAll : 전부 끝날 때 실행
  - server.close: 모든 테스트 실행 된 후 네이티브 요청 발행 모듈 복원(?)
- afterEach : 각 테스트가 끝날 때마다 실행
  - server.resetHandlers : 테스트 사이에 모든 요청 핸들러를 재설정

#### 4. 📁 src/mocks/ `server.ts` 파일 생성

```shell
  mkdir src/mocks

  touch src/mocks/server.ts
```

```ts
// src/mocks/server.ts
import { setupServer } from 'msw/node';

import { handlers } from './handlers'

const server = setupServer(...handlers);

export default server;
```

#### 5. 📁 src/mocks/ `handlers.ts` 파일 생성

```shell
  touch src/mocks/handlers.ts
```

```ts
import { rest } from 'msw';

const BASE_URL = 'http://localhost:3000';

const handlers = [
  rest.get(`${BASE_URL}`, (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json({ products }),
    );
  }),
];

export default handlers;
```

<br/>

### 👩🏻‍💻 Thinking in React 예제를 통해 REST API 모킹하기

#### 1. MSW 설치 및 파일 생성 및 설정 완료

```
├── package.json
├── package-lock.json
├── jest.config.js ✅
├── src
│   ├── App.tsx
│   ├── App.test.tsx ✅
│   ├── setupTests.ts ✅
│   ├── hooks 📁
│   │   ├── useFetchProducts.ts 
│   ├── mocks 📁
│   │   ├── handlers.ts ✅
│   │   └── server.ts ✅
```

#### 2. Thinking in React 예제의 Mock Date 기준으로 `handlers.ts` 작성

```ts
// src/mocks/handlers.ts 

import {rest} from 'msw';
// import fixtures from '../../fixtures';

const BASE_URL = 'http://localhost:3000';

const handlers = [
    rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
        // const { products } = fixtures; // fixtures를 사용해도 됨  
        const products = [
            {
                category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
            },
        ];

        return res(
            ctx.status(200), 
            ctx.json({products}), // 위의 요청을 이 형태로 반환
        );
    }),
];

export default handlers;
```

3. `App.test.tsx` 수정

- jest.mock 제거
- waitFor 추가 (~ 가 될 때까지 대기하는 상태)
- 콜백함수의 타입이 Promise로 되어 있어서 async/await 필요

```tsx
// App.test.tsx

import {render, screen} from '@testing-library/react';
import App from './App';

// jest.mock 불필요
// jest.mock('./hooks/useFetchProducts', () => () => fixtures.products);
//jest.mock('./hooks/useFetchProducts');

test('App', async () => {
    render(<App / >);

    await waitFor(() => {
        screen.getByText('Apple');
    });
});
```

#### 4. 🚨 Error 발생

```ts
// hooks/useFetchProducts.ts

export default function useFetchProducts() {
    const url = 'http://localhost:3000/products';
    const {data, error} = useFetch<ProductsResult>(url);
    console.log({error}); // 👈🏻 추가해서 확인해보니, Error
    if (!data) {
        return [];
    }

    return data.products;
}
```

> ReferenceError: `fetch is not defined` 발생

node.js 환경에서 테스트를 진행 중이었다. 최신버전의 node는 fetch를 지원하지만, 현재 사용 중인 버전의 Node는 지원하지 않아 나타난 error!
(💡 Fetch는 브라우저[window]에서만 지원)

#### 5. Github에서 만든 Fetch `Polyfill(폴리필)`을 사용해 해결

- Polyfill 설치

```shell
npm i -D whatwg-fetch
```

- `setupTests.ts` 파일 최상위에 import로 적용

```ts
import 'whatwg-fetch'
```

<br/>

## 🤔 [Polyfill(폴리필)](https://developer.mozilla.org/ko/docs/Glossary/Polyfill)은 무엇인가?

- 기본적으로 지원하지 않는 이전 브라우저에서 __최신 기능을 제공하는 데 필요한 코드__
- 새로운 문법을 낮은 버전에서도 돌아갈 수 있게 하기 위하여 만든 코드

#### 🔍 참고 할 수 있는 예시?

- 바벨과 같은 트랜스 파일러

> 최신 스펙으로 작성된 스크립트 코드를 명시한 버전에 따라 재작성 해줍니다.
낮은 버전의 자바스크립트 엔진에서도 프로그램이 돌아갈 수 있게 해주어 서비스가 문제없이 제공될 수 있게 해줍니다.
바벨에는 core-js 라이브러리가 탑재되어 es6 이후의 문법들에 폴리필을 이용할 수 있습니다.

<br/>

## 🔗 참고

- [Web Worker](https://velog.io/@whow1101/Web-Worker)
- [서비스 워커에 대해 알아보고 Mock Response 만들기](https://fe-developers.kakaoent.com/2022/221208-service-worker/)
- [Github에서 만든 Fetch polyfill(폴리필)](https://github.com/JakeChampion/fetch)
- [메가테라 참고 GitBook - Test fixture](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week5/msw)
- [⭐️ Mocking으로 생산성까지 챙기는 FE 개발](https://tech.kakao.com/2021/09/29/mocking-fe/)
- [⭐️ 똑똑하게 브라우저 Polyfill 관리하기](https://toss.tech/article/smart-polyfills)
- [Javascript Polyfill](https://dkrnfls.tistory.com/280)
