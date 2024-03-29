# 장바구니 페이지

## 장바구니

```
- CartPage
  - useFetchCart.tsx(ts)  // 👈🏻 장바구니 상품 목록 얻기 
  - Cart.tsx  // 👈🏻 장바구니 상품 목록 보여주기
    - LineItem.tsx
    - Options.tsx
```

### CartPage.tsx

- 장바구니로 추가한 상품을 보여주는 페이지  
- 장바구니 상품 목록을 서버로 부터 얻기 위해 `useFetchCart` hook 생성
- useFetchCart hook으로 부터 얻은 장바구니 상품목록을 `Cart` 컴포넌트로 구현

<br/>

### 장바구니 상품 목록 데이터를 얻기 위한 과정

- useFetchCart.ts 생성
- CartStore.ts 생성
  - Store 생성시 상태인 cart의 Type null 사용 →  Null Object 생성해서 적용

```ts
export const nullCart : Cart = {
  lineItems: [],
  totalPrice: 0,
};
```

- ApiService.ts `fetchCart` 추가

<br/>

### Cart.tsx

#### 🚨 `Cart` 타입과 `Cart` 컴포넌트 이름이 겹치는 문제 발생  

1. Cart 타입을 가져올 때 as CartType 사용해서 타입의 이름을 변경 ( ✅ 해당 방법 사용해서 처리 )
2. Cart 컴포넌트의 이름을 CartView 등 다른 이름 변경

<br/>

### LineItem.tsx

#### 🚨 `LineItem` 타입과 `LineItem` 컴포넌트 이름이 겹치는 문제 발생  

- `cart` 컴포넌트처럼 타입의 이름을 변경해서 사용

<br/>

### cURL을 이용해서 임의로 장바구니 상품 담기

```shell
curl -X POST https://shop-demo-api-01.fly.dev/cart/line-items \
  -H "Content-Type: application/json" \
  --data '
    {
      "productId": "0BV000PRO0001",
      "options": [
        {
          "id": "0BV000OPT0001",
          "itemId": "0BV000ITEM001"
        },
        {
          "id": "0BV000OPT0002",
          "itemId": "0BV000ITEM005"
        }
      ],
      "quantity": 1
    }
  '
```

### 📖 curl (Client URL)

- URL로 데이터를 전송하여 서버에 데이터를 보내거나 가져올때 사용하기 위한 명령줄 도구 및 라이브러리
- Shell(커맨드라인환경)에서 REST API 테스트하고 싶을 경우 사용되는 도구
- 리눅스기반 따로 설치 없이 사용 가능 / Window 경우 설치 필요 → [Postman](https://www.postman.com/)

<br/>

### 임의 테스트 초기화 작업

- backdoor/setup-database

<br/>

## 🔗 참고

- [CURL 명령어 사용법 💯 완전 총정리](https://inpa.tistory.com/entry/LINUX-%F0%9F%93%9A-CURL-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%8B%A4%EC%96%91%ED%95%9C-%EC%98%88%EC%A0%9C%EB%A1%9C-%EC%A0%95%EB%A6%AC)
- [Mac book에서 커맨드라인 환경에서 REST API (HTTP) 요청 보내기](https://velog.io/@ryan_95/Mac-book에서-커맨드라인-환경에서-REST-API-HTTP-요청-보내기)
