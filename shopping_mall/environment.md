# 개발하기 전 준비

## 📖 용어

- `Product` : 상품
  - `Summary` : 상품에 대한 요약 정보
  - `Detail` : 상품에 대한 상세 정보
  - `Image` : 상품 이미지
  - `Option` : 상품에 대한 상세 옵션 종류 (색상, 크기 등)
  - `OptionItem` : 옵션에 대한 상세 옵션 값 (옵션이 색상이라면 이건 Blue, Red 의미)
- `Category` : 상품에 대한 분류
- `Cart` : 장바구니
  - `LineItem` : 장바구니에 담긴 것
    - 상품, 옵션, 수량 등을 동시에 다룸
    - 여기서도 Option과 OptionItem을 사용한다.

> 용어는 동일하지만 Product와 다른 구성을 갖기 때문에, Product와 Order라는 접두어를 활용한다. <br/>
시스템을 분리할 수 있다면, 근본적으로 나누는 걸 추천 (상품 정보 확인 / 장바구니 / 주문).

- `Order` : 주문
  - 여기서도 동일한 LineItem 활용.
  - Detail: 주문에 대한 상세 정보
  - Cart와 동일한 LineItem을 활용한다(오히려 Cart가 Order의 LineItem을 활용하는 것에 가깝다).
- `User` : 사용자

<br/>

## 🤖 기능

> 기능구현 순서는 `비즈니스`의 우선 순위에 따라 결정

1. 상품 목록 확인
2. 상품 상세 정보 확인
3. 장바구니에 상품 담기
4. 로그인
5. 로그아웃
6. 회원 가입
7. 주문 목록 확인
8. 주문 상세 확인
9. 주문하기 → 배송지 입력, 결제

<br/>

## 🖥️ 화면

- 홈 페이지 : `/`
- 상품 목록 페이지 : `/products`
- 상품 상세 페이지 : `/products/{id}`
- 장바구니 페이지 : `/cart`
- 주문 페이지 : `/order`
- 주문 완료 페이지 : `/order/complete`
- 주문 목록 페이지 : `/orders`
- 주문 상세 페이지 : `/orders/{id}`
- 로그인 페이지 : `/login`
- 회원 가입 페이지 : `/signup`
- 회원 가입 완료 페이지 : `/signup/complete`

<br/>

## 🌐 REST API

- [📄 온라인 쇼핑몰 API](https://docs.google.com/document/d/1bGYl3IDoX53cNBbZHNlsRhPLZQ3Qiu-Jm3gpqyu_xI0/view)
- [📄 인증/인가가 적용된 온라인 쇼핑몰 API](https://docs.google.com/document/d/1bGYl3IDoX53cNBbZHNlsRhPLZQ3Qiu-Jm3gpqyu_xI0/view)
- `API Base URL` [https://shop-demo-api-01.fly.dev/](https://shop-demo-api-01.fly.dev/)
- `인증/인가 적용된 API Base URL` [https://shop-demo-api-02.fly.dev/](https://shop-demo-api-02.fly.dev/)

<br/>

## ⚙️ 개발 환경 세팅

- [📄 개발환경 세팅 가이드](https://github.com/megaptera-kr/textbook/tree/main/start-megaptera-shop-client)

<br/>

### 세팅 목록

- TypeScript
- ESLint
- React
- Parcel

#### Routing

- [React Router](https://github.com/remix-run/react-router)

#### CSS

- [styled-components](https://github.com/styled-components/styled-components)
- [styled-reset](https://github.com/zacanger/styled-reset)

#### Hook

- [usehooks-ts](https://github.com/juliencrn/usehooks-ts)

#### API

- [Axios](https://github.com/axios/axios)
  - REST API 사용을 위한 HTTP 클라이언트

#### External Store

- [tsyringe](https://github.com/microsoft/tsyringe)
- [reflect-metadata](https://github.com/rbuckton/reflect-metadata)
- [usestore-ts](https://github.com/seed2whale/usestore-ts)

#### Test

- Jest
- React Testing Library
  - [jest-dom](https://github.com/testing-library/jest-dom)
    - React Testing Library에서 활용할 수 있는 DOM 확인용 Matcher 모음
    - 테스트 코드의 의미를 더욱 명확히 하기 위해 사용
- [MSW](https://github.com/mswjs/msw)
- [CodeceptJS](https://github.com/codeceptjs/CodeceptJS) : E2E 테스트시 사용

<br/>

### 📄 Test Helper

- 테스트 코드에서 __styled-components의 Theme과 React Router의 Link 등을 사용할 때 문제가 발생하지 않도록,__ React Testing Library의 render를 한번 감싼 `테스트용 헬퍼 함수`를 준비.

```tsx
// utils/test-helpers.tsx

import { render as originalRender } from '@testing-library/react';

import React from 'react';

import { MemoryRouter } from 'react-router-dom';

import { ThemeProvider } from 'styled-components';

import defaultTheme from './styles/defaultTheme';

export function render(element: React.ReactElement) {
  return originalRender((
    <MemoryRouter initialEntries={['/']}>
      <ThemeProvider theme={defaultTheme}>
        {element}
      </ThemeProvider>
    </MemoryRouter>
  ));
}
```

<br/>

### 📄 Types

- 기본적인 타입 준비
- 미리 정한 용어집과 REST API 스펙에 맞게 설정
- [🔗 types](https://github.com/megaptera-kr/textbook/blob/main/start-megaptera-shop-client/src/types.ts)

> 해당 실습에서는 types.ts 파일에 모든 타입을 정의

<br/>

### 📄 MSW 세팅

- REST API 스펙에 맞춰서 MSW `핸들러(handlers)` 준비

```ts
// src/mocks/handlers.ts

import { rest } from 'msw';

import { ProductSummary } from '../types';

import fixtures from '../../fixtures';

const BASE_URL = 'https://shop-demo-api-01.fly.dev';

const productSummaries: ProductSummary[] = fixtures.products
  .map((product) => ({
    id: product.id,
    category: product.category,
    thumbnail: product.images[0],
    name: product.name,
    price: product.price,
  }));

const handlers = [
  rest.get(`${BASE_URL}/categories`, (req, res, ctx) => (
    res(ctx.json({ categories: fixtures.categories }))
  )),
  rest.get(`${BASE_URL}/products`, (req, res, ctx) => (
    res(ctx.json({ products: productSummaries }))
  )),
  rest.get(`${BASE_URL}/products/:id`, (req, res, ctx) => {
    const product = fixtures.products.find((i) => i.id === req.params.id);
    if (!product) {
      return res(ctx.status(404));
    }
    return res(ctx.json(product));
  }),
  rest.get(`${BASE_URL}/cart`, (req, res, ctx) => (
    res(ctx.json(fixtures.cart))
  )),
  rest.post(`${BASE_URL}/cart/line-items`, (req, res, ctx) => (
    res(ctx.status(201))
  )),
];

export default handlers;
```
