# 상품 목록 페이지

## 상품 목록

```
- ProductListPage.tsx
  - useFetchProducts.ts  // 👈🏻 store로 부터 상품 목록 얻어 반환 
    - ProductsStore.ts  // 👈🏻 store 생성 
      - ApiService.ts  // 👈🏻 API 호출 모아서 관리하는 파일
  - Products.tsx  // 👈🏻 상품 목록 리스트 
    - Product.tsx // 👈🏻 개별 상품 
      - numberFormat.ts // 👈🏻 유틸리티 함수
```

### `ProductListPage.tsx`

- 상품 목록을 얻어 상품 리스트를 보여주는 페이지

### `useFetchProducts.ts`

- __ProductsStore.ts__ 부터 상품목록에 대한 상태와 API 요청 함수 호출 후 상품목록을 반환하는 hook

### `ProductsStore.ts`

- 상품 목록에 대한 상태, API 요청 액션 함수로 구성

### `ApiService.ts`

- API 호출을 모아 모아서 관리하는 파일

### `Products.tsx`

- props로 전달받은 상품 목록 리스트 (__products__) 정보로 UI 구현

### `Product.tsx`

- props로 개별 상품의 정보로 UI구현

### `numberFormat.ts`

- 가격의 숫자를 읽기 좋게 보여주기 위해 유틸리티 함수 작성

```ts
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}
```

#### 📖 [Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)

> 다국어 지원을 할 때 유용하게 쓸수 있는 Intl API

#### [Intl.NumberFormat](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat)

- 언어에 맞는 숫자 서식을 지원하는 객체의 생성자

<br/>

## 카테고리 목록

```
- Header
  - useFetchCategories.ts  // 👈🏻 카테고리 목록 얻기
  - category.tsx  // 👈🏻 카테고리 보여주기
```

<br/>

## 카테고리별 상품 목록

### 카테고리 ID를 얻기 위해 `useSearchParams` hooks 사용

- `ProductListPage.tsx` 컴포넌트에서 hooks 사용

```jsx
export default function ProductListPage() {
  const [params] = useSearchParams();

  const categoryId = params.get('categoryId') ?? undefined;

  // TODO : 1.상품 목록 얻기
  const { products } = useProducts({categoryId});

  // TODO : 2. 상품 목록 보여주기
  return (
    <article>
      <h2>상품 목록 페이지</h2>
      <Products products={products} />
    </article>
  );
}
```

### 전달받은 categoryId 사용하도록 변경

- 해당 파일들에 categoryId 사용될 수 있도록 인수 전달 및 타입 지정
- useProducts.ts
- ProductsStore.ts
- ApiService.ts
  - axios `params` 옵션을 통해 categoryId 전달

```ts
const { data } = await this.instance.get('/products', { params: { categoryId } });
```

<br/>

## 🔗 참고

- [자바스크립트의 Intl API로 국제화하기](https://www.daleseo.com/js-intl-api/)
- [Intl.NumberFormat 이용한 나라별 통화 표기하기](https://peter-codinglife.tistory.com/67)
- [[Axios] axios 사용시 params는 어떻게 사용할까?](https://velog.io/@minsu2344/Axios-axios-사용시-params는-어떻게-사용할까)
