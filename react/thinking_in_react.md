# Thinking in React

## 학습 내용

   리액트 공식 문서에 소개된 🔗 [Thinking in React](https://ko.legacy.reactjs.org/docs/introducing-jsx.html) 예제를 통한 코드 실습

### `step 0` Start with the mockup

- 개발 환경 세팅
- Thinking in React 예제 Static Version UI 구현 (퍼블리싱 작업)

![UI](./image/thinking-in-react_ui.png)

### `step 1` Break the UI into a component hierarchy

- UI를 구성 요소 계층 구조로 나누기 (컴포넌트 구성)

- `FilterableProductTable`
  - `SearchBar`
  - `ProductTable`
    - `ProductCategoryRow`
    - `ProductRow`

![Component](./image/thinking-in-react_ui_component.png)

### `step 2` Build a static version in React

- UI를 렌더링 하는 버전을 구축 (props 선언 및 전달)

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
