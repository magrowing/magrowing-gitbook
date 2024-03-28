# 상품 상세 페이지

## 상품 상세

```
- ProductDetailPage
  - useFetchProduct.ts  // 👈🏻 loading, error 대한 처리 hook
  - ProductDetail.tsx  // 👈🏻 Store 통해 상품 상세 얻기
```

### ProductDetailPage.tsx

- 상품 상세 정보를 얻어서 보여주는 페이지
- useFetchProduct 통해서 상품을 찾을 수 없는 경우를 따로 구분하진 않고 일반 Error로 구현
- 장바구니 담기 기능 때문에 `Prop drilling` 문제가 발생 될 수 있어, prop로 상품상세정보 받기보다는 store 통해 전달

<br/>

## Loading, Error UI 구현

### `useParams` hook을 사용해서 productId 얻어오기

```jsx
export default function ProductDetailPage() {
  const params = useParams(); // 👈🏻 hooks

  const { loading, error } = useFetchProduct({
    productId: String(params.id),
  });

  if (loading) {
    return (
      <p>Loading...</p>
    );
  }

  if (error) {
    return (
      <p>Error!</p>
    );
  }

  return (
    <ProductDetail />
  );
}
```

<br/>

### useFetchProduct.ts

- 스토어에서 loading 과 error 값을 반환하는 custom hook 역활

<br/>

### ProductDetailStore.ts

- 상품상세정보, loading, error 값을 가지고 있는 Store

#### Null Object 생성

- 상품정보의 타입에 맞게 `types.ts` 파일 안에  NullObject 생성
- [Special Case](https://refactoring.com/catalog/introduceSpecialCase.html)

```ts
@singleton()
@Store()
export default class ProductDetailStore {
  product: ProductDetail = nullProductDetail;

  loading = true;

  error = false;

  async fetchProduct({ productId }: {
    productId: string;
  }) {
    this.startLoading();

    try {
      const product = await apiService.fetchProduct({ productId });
      this.setProduct(product);
    } catch {
      this.setError();
    }
  }

  @Action()
  private startLoading() {
    this.product = nullProductDetail;
    this.loading = true;
    this.error = false;
  }

  @Action()
  private setProduct(product: ProductDetail) {
    this.product = product;
    this.loading = false;
    this.error = false;
  }

  @Action()
  private setError() {
    this.product = nullProductDetail;
    this.loading = false;
    this.error = true;
  }
}
```

### useProductDetailStore.ts

- `ProductDetailStore`를 사용해서 스토어에 접근 할 수 있는 hook

#### ✅ Refactor

- `useFetchProduct.ts` 분리 작업

```ts
export default function useProductDetailStore() {
  const store = container.resolve(ProductDetailStore);
  return useStore(store);
}
```

<br/>

## 상품 상세 정보 UI 구현

### ProductDetail.tsx

- store 에서 상품 정보 얻어오기
- UI 구현
  - `Images` : 상품 이미지
  - `AddToCartForm` : 장바구니 추가되는 영역
  - `Description` : 상품 정보에 대한 문구

<br/>

### Images.tsx

```ts
const Thumbnail = styled.img.attrs({
  alt: 'Product Image',
})`
  display: block;
  width: 100%;
  aspect-ratio: 1/1;
`;

type ImagesProps = {
  images: Image[];
}

export default function Images({ images }: ImagesProps) {
  const [image] = images;

  if (!image) {
    return null; // 👈🏻 이미지가 없을 경우 더미 이미지를 준비하면 좋을 듯 
  }

  return (
    <Thumbnail src={image.url} />
  );
}
```

<br/>

### Description.tsx

```ts
type DescriptionProps = {
  value: string;
};

export default function Description({ value }: DescriptionProps) {
  return <div>{JSON.stringify(value)}</div>;
}
```

> "Color: Black, Grey, Blue, Brown\nSize: ONE\n\n편하게 입을 수 있는 맨투맨\n고급 원단과 봉제가 들어가\n유니크함이 돋보이는 디자인\n내츄럴한 멋을 살리는 핏"

#### 여러줄로 처리된 데이터를 줄바꿈 처리

```jsx
type DescriptionProps = {
  value: string;
};

export default function Description({ value }: DescriptionProps) {

  if (!value) {  // 👈🏻 value 없을 경우 
    return null;
  }

  const lines = value.split('\n'); // 👈🏻 \n 기준으로 배열 생성 

  return (
    <div>
      <div>
        {lines.map((line) => (
          <p key=key(line, index)</p>
        ))}
      </div>
    </div>
  );
}
```

```jsx
function key(text: string, index: number) { // 👈🏻 key 꼼수
  return `${text}-${index}`;
}

export default function Description({ value }: DescriptionProps) {
  if (!value) {
    return null;
  }
  const lines = value.split('\n');
  return (
    <div>
      <div>
        {lines.map((line, index) => (
          <p key={key(line, index)}>{line}</p>
        ))}
      </div>
    </div>
  );
}
```
