# Thinking in React

## 학습 내용

   리액트 공식 문서에 소개된 🔗 [Thinking in React](https://ko.legacy.reactjs.org/docs/introducing-jsx.html) 예제를 통한 코드 실습

<br/>

## Thinking in React

### 1. `step 0` Start with the mockup

- 개발 환경 세팅 (vite)

  ```shell
   npm create vite@latest 
  ```

- Thinking in React 예제의 전제 조건 및 Mockup 파악
  - Mockup 과 JSON 형식의 API 확인 후 ⇒ 사용자가 볼 수 있도록 UI 설계

![UI](./image/thinking-in-react_ui.png)

### 2. `step 1` Build a static version in React

1. Static Version UI 구현 (퍼블리싱 작업)
2. JSON 에서 카테고리의 value 값을 추출  ⇒ reduce 메서드를 통해

  ``` json
    [
      { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
      { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
      { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
      { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
      { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
      { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
    ]
  ```

  ```tsx
    const categories = products.reduce(
      (acc: string[], product: Product) =>
        acc.includes(product.category) ? acc : [...acc, product.category],
      []
    );
  ```

### 3. `step 1` Break the UI into a component hierarchy

- UI를 구성 요소 계층 구조로 나누기 (컴포넌트 구성)

1. JSON API 기반으로 UI 구현하니, 반복적인 요소가 보여지고 코드가 길어짐 ⇒ table tbody 영역 컴포넌트로 분리 `ProductsInCategory`
2. `ProductInCategory`컴포넌트를 분리하고 보니, 반복적인 요소 발견 ⇒ `ProductRow` `ProductCategoryRow` 컴포넌트 분리
3. `ProductTable` 컴포넌트를 생성해서 table 영역 전체 맵핑
4. 검색 영역 `SearchBar` 컴포넌트로 분리
5. `FilterableProductTable` table 과 검색 영역 전체 맵핑
6. `SearchBar` 분리하고 field `CheckBoxField` 영역 추가 분리

- `5.FilterableProductTable`
  - `4.SearchBar`
    - `6.CheckBoxField`
  - `3.ProductTable`
    - `1.ProductsInCategory`
      - `2.ProductCategoryRow`
      - `2.ProductRow`

![Component](./image/thinking-in-react_ui_component.png)

### `step 3` Find the minimal but complete representation of UI state

- state(상태)가 필요한 부분을 식별
  - 원래 제품 목록 (props)
  - 사용자가 입력한 검색어 (state) `SearchBar`
  - 체크박스의 값 (state) `SearchBar`
  - 필터링된 제품 목록 (props)

### `step 4` Identify where your state should live

- state(상태)를 식별 한 후 state(상태)의 공통 상위 요소를 찾아 state(상태)의 위치 결정

### `step 5` Add inverse data flow

- state(상태)를 업데이트 하기 위한 콜백함수를 역방향으로 끌어올리기

<br/>
