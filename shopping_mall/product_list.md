# 상품 목록 페이지

## 상품 목록

```
- ProductListPage
  - useFetchProducts.tsx(ts)  // 👈🏻 상품 목록 얻기
  - Products.tsx  // 👈🏻 상품 목록 보여주기
    - Product.tsx // 👈🏻 개별 상품 보여주기
```

### ProductListPage.tsx

- 상품 목록을 얻어 상품 리스트를 보여주는 페이지
- 상품 목록을 서버로 부터 얻기 위해 `useFetchProducts` hook 생성
- useFetchProducts hook으로 부터 얻은 상품목록을 `Products` 컴포넌트로 구현

<br/>

### useFetchProducts

- tsx : axios 사용, `GET /products` 요청으로 응답받은 상품 목록 리스트 반환
- ts : Store로부터 상품목록을 받아 관리하는 hook으로 변경

<br/>

### Products.tsx

- `JSON.stringify` 를 통해 데이터 확인
- props 로 상품 목록 리스트 배열 전달 받는 역활

<br/>

### Product.tsx

- 상품 1개의 정보를 담고 있는 역활
- 가격의 숫자를 읽기 좋게 보여주기 위해 `numberFormat` 유틸 함수로 구현

```ts
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}
```

### 📖 [Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)

- 다국어 지원을 할 때 유용하게 쓸수 있는 Intl API

#### [Intl.NumberFormat](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat)

- 언어에 맞는 숫자 서식을 지원하는 객체의 생성자

<br/>

### 상품 목록은 Store로 관리

- `ProductsStore.ts` : 서버부터 응답받은 반환값 상품목록 가지고 있는 Store

```ts
@singleton()
@Store()
export default class ProductsStore {
  products: ProductSummary[] = [];

  async fetchProducts() { 
    this.setProducts([]);

    const { data } = await axios.get(`${apiBaseUrl}/products`); // 👈🏻 API요청 
    const { products } = data;

    this.setProducts(products);
  }

  @Action()
  setProducts(products: ProductSummary[]) {
    this.products = products;
  }
}
```

<br/>

## 카테고리 목록

```
- Header
  - useFetchCategories.tsx(ts)  // 👈🏻 카테고리 목록 얻기
  - category.tsx  // 👈🏻 카테고리 보여주기
```

### 카테고리 목록을 Store로 관리

- `CategoriesStore.ts` : 서버부터 응답받은 반환값 카테고리 목록을 가지고 있는 Store

```ts
@singleton()
@Store()
export default class CategoriesStore {
  categories: Category[] = [];

  async fetchCategories() {
    this.setCategories([]);

    const { data } = await axios.get(`${apiBaseUrl}/Categories`); // 👈🏻 API요청 
    const { categories } = data;

    this.setCategories(categories);
  }

  @Action()
  setCategories(categories: Category[]) {
    this.categories = categories;
  }
}
```

### useFetchCategories

- tsx : axios 사용, `GET /categories` 요청으로 응답받은 카테고리 목록 리스트 반환
- ts : Store로부터 상품목록을 받아 관리하는 hook으로 변경

<br/>

### ✅ Refactor

- 상품목록을 관리 하는 Store 와 카테고리 목록을 관리하는 Store 의 중복코드 제거
  - apiBaseUrl
  - Store 와 Axios의 관계

#### 1. `services` 폴더 및 `ApiService.ts` 생성

- API 호출을 모아주는 역활
- API의 base URL을 지정하기 위해 환경변수를 활용

```ts
import axios from 'axios';

import { Category, ProductSummary } from '../types';

const API_BASE_URL = process.env.API_BASE_URL
                      || 'https://shop-demo-api-01.fly.dev';

export default class ApiService {
  private instance = axios.create({
    baseURL: API_BASE_URL,
  });

  async fetchCategories(): Promise<Category[]> {
    const { data } = await this.instance.get('/categories');
    const { categories } = data;
    return categories;
  }

  async fetchProducts({ categoryId } :{categoryId? : string} = {}): Promise<ProductSummary[]> {
    const { data } = await this.instance.get('/products', { params: { categoryId } });
    const { products } = data;
    return products;
  }
}

export const apiService = new ApiService();
```

#### 2.Store 파일에서 axios API 호출 → `apiService` 변경

- useProducts.ts
- useCategories.ts

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
