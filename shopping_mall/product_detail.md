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
  - `Description` : 상품 정보에 대한 문구
  - `AddToCartForm` : 장바구니 추가되는 영역

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

<br/>

## 장바구니 상품 담기

> 🤔 장바구니에 상품을 담는다는 의미는? <br/>
`Product`와 관련된 `Option` 정보, 수량 등 다양한 값이 조합돼 `Cart`의 `LineItem`을 구성하는 것  

실제로는 조금 복잡한 도메인 로직이 들어갈 수 있는데, 이런 처리는 백엔드에서 담당하기로 하고, 우선은 상품과 관련된 옵션, 수량, 가격, 담는 버튼에만 집중해서 구현

<br/>

### AddToCartForm.tsx

- Props Drilling을 피하기 위해 개별 컴포넌트 Store 사용  

```jsx
export default function AddToCartForm() {
  return (
    <div>
      <Options />
      <Quantity />
      <Price />
      <SubmitButton />
    </div>
  );
}
```

- `Options` : 옵션을 보여주고, 선택할 수 있게 하는 영역
- `Quantity` : 수량 버튼 (기본값 : 1)
- `Price` : 수량에 맞는 가격 출력
- `SubmitButton` : 장바구니 담기 버튼, 담았다는 메세지 출력

<br/>

### AddToCartForm.test.tsx

- MSW 활용한 방식

```jsx
import { fireEvent, screen, waitFor } from '@testing-library/react';

import { container } from 'tsyringe';

import { render } from '../../../utils/test-helpers';

import AddToCartForm from './AddToCartForm';

import ProductDetailStore from '../../../stores/ProductDetailStore';

import fixtures from '../../../../fixtures';

test('AddToCartFore', async () => {
  // MWC
  container.clearInstances();
  const [product] = fixtures.products;
  const productDetailStore = container.resolve(ProductDetailStore);
  await productDetailStore.fetchProduct({ productId: product.id });

  render(<AddToCartForm />);

  fireEvent.click(screen.getByText('장바구니에 담기'));

  await waitFor(() => {
    screen.getByText(/장바구니에 담았습니다./);
  });
});
```

<br/>

### Store 및 Store 사용을 위한 hooks 생성

- useProductFormStore.ts
- ProductFormStore.ts
- ProductFormStore.test.ts
  - 사소한 비즈니스 로직이지만, 테스트 코드를 통해 검증

<br/>

### Quantity : 수량 버튼 기능 구현

- Quantity.tsx
  - `useProductFormStore`를 통해 quantity, changeQuantity 함수를 사용해서 구현
- Quantity.test.tsx
- ProductFormStore.test.ts

  ```ts
  describe('changeQuantity', () => {
      context('with correct value', () => {
        it('changes quantity', () => {
          store.changeQuantity(3);

          expect(store.quantity).toBe(3);
        });
      });
      context('with correct value', () => {
        it("doesn't change quantity", () => {
          store.changeQuantity(-1);
          store.changeQuantity(11);

          expect(store.quantity).toBe(1);
        });
      });
    });
  ```

<br/>

### Price : 수량에 맞는 가격 UI 구현

```jsx
// price.tsx

export default function Price() {
  const [{ product }] = useProductDetailStore();
  const [{ quantity }] = useProductFormStore();

  return (
    <Container>
      {numberFormat(product.price * quantity)}
      원
    </Container>
  );
}
```

```jsx
// price.test.tsx

import { screen } from '@testing-library/react';

import Price from './Price';

import { render } from '../../../utils/test-helpers';

import fixtures from '../../../../fixtures';

const [product] = fixtures.products;
const { options } = product;

jest.mock('../../../hooks/useProductDetailStore', () => () => [
  {
    product: { ...product, price: 123_000 },
  },
]);

jest.mock('../../../hooks/useProductFormStore', () => () => [
  {
    options,
    selectedOptionItems: options.map((i) => i.items[0]),
    quantity: 2,
  },
]);

describe('Price', () => {
  it('renders price as formatted number', () => {
    render(<Price />);
    screen.getByText(/246,000원/);
  });
});
```

#### Getter 이용한 방식

- `ProductFormStore` store 수량에 따른 금액을 계산하는 메서드 또는 Getter가 있다면 다른 형태로 접근 가능

```jsx
// price.tsx
import { useEffect } from 'react';

import useProductDetailStore from '../../../hooks/useProductDetailStore';
import useProductFormStore from '../../../hooks/useProductFormStore';

import numberFormat from '../../../utils/numberFormat';

export default function Price() {
  const [{ product }] = useProductDetailStore();
  const [, productFormStore] = useProductFormStore();

  useEffect(() => {
    productFormStore.setProduct(product);
  }, [product, productFormStore]);

  return (
    <div>
      <dl>
        <dt>{`${numberFormat(product.price)}원`}</dt>
        <dd>{`결제 금액 : ${numberFormat(productFormStore.price)}원`}</dd>
      </dl>
    </div>
  );
}
```

```jsx
// price.test.tsx

import { container as iocContainer } from 'tsyringe';

import { render } from '../../../utils/test-helpers';

import Price from './Price';

import ProductFormStore from '../../../stores/ProductFormStore';

import numberFormat from '../../../utils/numberFormat';

import fixtures from '../../../../fixtures';

const [product] = fixtures.products;

jest.mock('../../../hooks/useProductDetailStore', () => () => [
  {
    product: { ...product, price: 123_000 },
  },
]);

describe('Price', () => {
  const quantity = 2;

  beforeEach(() => {
    const productFormStore = iocContainer.resolve(ProductFormStore);
    productFormStore.changeQuantity(quantity);
  });

  it('renders price as formatted number', () => {
    const { container } = render(<Price />);
    const price = numberFormat(product.price * quantity);
    expect(container).toHaveTextContent(`${price}원`);
  });
});
```

```ts
// ProductFormStore.ts

@singleton()
@Store()
export default class ProductFormStore {
  product : ProductDetail = nullProductDetail;

  quantity = 1;

  @Action()
  setProduct(product : ProductDetail) {
    this.product = product;
  }

  // 중략 

  get price() { // 👈🏻 Getter 
    return this.product.price * this.quantity;
  }
}
```

#### 📖 Getter 와 Setter

- Getter(획득자)
- Setter(설정자)
  - 객체 리터럴 안에서는 `get`과 `set`으로 나타낼 수 있음.

> get : 인수가 없는 함수로, 프로터피를 읽을 때 동작함

<br/>

### SubmitButton : 장바구니 담기 버튼 기능 구현

#### SubmitButton.tsx

- `useProductFormStore` store로 부터 done 과 store.addToCart 함수를 적용

#### SubmitButton.test.tsx

- store에 대해 mocking을 통해 통과했지만 실제 SubmitButton은 구현해두지 않았다.
  - mocking의 약점! 주의하자!

#### ProductFormStore의 싱태 추가 및 액션 addToCart 추가  

```ts
  product : ProductDetail = nullProductDetail;

  selectedOptionItems : ProductOptionItem[] = [];

  done = false;

  async addToCart(){
    // 서버 통신 
  }

  @Action()
  setProduct(product : ProductDetail) {
    // 초기 상품에 대한 설정 
  }

  @Action()
  resetDone() {
    // 초기 장바구니에 관해 false 처리 
    this.done = false;
  }

  @Action()
  complete() {
    // 장바구니 추가 버튼 클릭 후 수량 및 장바구니에 관한 true 처리 
    this.quantity = 1;
    this.done = true;
  }
```

<br/>

### ✅ Refactor

#### price.tsx

- product 변경에 따른 setProduct 호출은 여기가 아니라 page 등에서 처리
- `useFetchProduct.ts`로 이동
- `price.test.tsx` :  Mocking 제거 MSW로 실제로 스토어에서 받아옴

```jsx
  // useFetchProduct.ts

  const [{ product, loading, error }, productDetailStore] = useProductDetailStore();
  const [_, productFormStore] = useProductFormStore(); // 👈🏻 추가 

  useEffect(() => {  
    productDetailStore.fetchProduct({ productId });
  }, [productDetailStore, productId]);

  useEffect(() => { // 👈🏻 추가 
    productFormStore.setProduct(product);
  }, [product, productFormStore]);
```

<br/>

### Options : 옵션 선택 기능 구현

```
- Options.tsx
  - Option.ts  
    - ComboBox.tsx // 범용 
- ProductFormStore.ts // `changeOptionItem` 추가 
```

<br/>

## 🔗 참고

- [자바스크립트의 프로퍼티 getter와 setter](https://ko.javascript.info/property-accessors)
