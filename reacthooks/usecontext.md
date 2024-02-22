# useContext

## 학습 키워드

- useContext

<br/>

## 📖 [useContext](https://react.dev/reference/react/useContext)

- `Context`로 분류한 데이터를 쉽게 받아올 수 있게 도와주는 역할하는 Hook

> 리액트의 일반적인 데이터 흐름은 부모 컴포넌트에서 자식 컴포넌트로 props를 통해 '단방향'으로 흐른다. 엄청 큰 컴포넌트 트리가 있다고 가정할 때 공통적으로 필요한 전역적인 데이터가 있을 수 있다. 예를 들어 GlobalData인 전역 데이터를 일일이 props로 단계별로 전달해야 한다면 문제들이 발생한다.

- 중간 컴포넌트에서 데이터를 변환하여 전달할 가능성이 있다.
- props를 계속 전달해주어야 하기 때문에 컴포넌트 가독성이 떨어진다.
- 컴포넌트가 다른 컴포넌트와 강한 의존성을 가질 수 있다.

리액트는 이러한 문제점을 해결해 주는 Context API를 제공한다. `Context`는 앱 안에서 __전역적으로 사용되는 데이터를__ 여러 컴포넌트끼리 쉽게 공유할 수 있는 방법을 제공한다. Context를 사용하면 Props로 데이터를 일일이 전달해 주지 않아도 상위 컴포넌트의 data가 필요한 하위 컴포넌트들은 useContext 훅을 사용해서 해당 데이터를 받아오기만 하면 된다.

#### 🤔 Context를 사용하기 전에 고려할 것

- Context API는 전역적으로 상태를 __공유해주는 기능만 수행__ 한다.

상위 컴포넌트의 Context가 만약 변경되는 state 값이라면 state값이 변경되어 리렌더링 일어나게 된다. 그렇다면 하위 컴포넌트까지 리렌더링이 되는 이슈가 있고, 이를 방지하기 위해서 React.memo 를 통해 최적화를 해줘야 한다. 또한 Context를 사용하면 컴포넌트를 재사용하기가 어려워지므로 꼭 필요할 때만 사용 해야 한다.

Context의 주된 목적인 다양한 레벨이 있는 많은 컴포넌트들에게 전역적인 데이터를 전달하기 위함이다. 리액트 공식 문서에는 Context를 사용하는 이유가 단지 Prop Drilling을 피하기 위한 목적이라면 Component Composition(컴포넌트 합성)이 더 간단한 해결책이라고 제시하고 있다.

#### 🤔 그럼 어떤 상황에서 사용 될 수 있을까?

- __상태변화가 거의 일어나지 않을 것으로__ 예상되는 __테마와 같은 값을__ 전달하기에 적합

### 🤖 useContext 사용법

#### 1. React.createContext

Context 객체를 생성한다. `defaultValue` 매개변수는 트리 안에서 적절한 Provider를 찾지 못했을 때만 쓰이는 값이다.

```jsx
import { createContext } from "react";

const MyContext = React.createContext(defaultValue);
```

#### 2. Context.Provider

Provider는 value prop을 받아서 이 값을 하위에 있는 컴포넌트에게 전달한다.

```jsx
import { MyContext } from "./context/MyContext";

<MyContext.Provider value={/* 어떤 값 */}>
```

<br/>

### 🔗 참고

- [다시 한번 useContext를 파헤쳐보자](https://velog.io/@hjthgus777/React-다시-한번-useContext를-파헤쳐보자)
- [useContext 조금 더 자세히 사용해보기](https://hokeydokey.tistory.com/205)
