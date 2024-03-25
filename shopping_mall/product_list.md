# 상품 목록 페이지

## 상품 목록

```
- ProductListPage
  - useFetchProducts.tsx(ts)  // 👈🏻 상품 목록 얻기
  - Products.tsx  // 👈🏻 상품 목록 보여주기
    - Product.tsx // 👈🏻 개별 상품 보여주기
```

### ProductListPage

- 상품 목록을 얻어 상품 리스트를 보여주는 페이지
- 상품 목록을 서버로 부터 얻기 위해 `useFetchProducts` hook 생성
- useFetchProducts hook으로 부터 얻은 상품목록을 `Products` 컴포넌트로 구현

<br/>

### useFetchProducts

- axios 사용해서 `GET /products` 요청해서 상품 목록 리스트 반환

<br/>

## 카테고리 목록

```
- Header
  - useFetchCategories.tsx(ts)  // 👈🏻 카테고리 목록 얻기
  - category.tsx  // 👈🏻 카테고리 보여주기
```

### useFetchCategories

- axios 사용해서 `GET /categories` 요청해서 카테고리 목록 리스트 반환

### Store를 통해 리스트 반환

<br/>

## 카테고리별 상품 목록
