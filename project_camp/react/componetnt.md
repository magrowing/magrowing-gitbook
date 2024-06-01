# Component

## 학습 키워드

- Component
  - 클래스 컴포넌트와 함수형 컴포넌트
  - 컴포넌트 생성 규칙
  - JSX
  - Props의 활용법

<br/>

## Component

리액트는 웹의 구성요소를 **가장 작은 단위로** 인 컴포넌트로 구성한다. 이러한 컴포넌트를 조합해서 하나의 페이지로 생성하고, 반복적인 웹의 구성요소를 재사용 할 수 있다는 특징을 가지고 있다.

<br/>

### 클래스 컴포넌트 와 함수형 컴포넌트

리액트는 클래형과 함수형 컴포넌트 두가지 방식으로 작성할 수 있다. 두가지 방식 모두 지원하고 있지만, 공식적으로는 클래스 컴포넌트 방식은 사용하지 않는걸 권장한다. 리액트 16.8버전에 추가된 `리액트 훅`으로 인해 함수형 컴포넌트의 사용성이 개선되어 더이상 클래스 컴포넌트를 사용하는 건 비효율적이다.

<br/>

### 컴포넌트 생성 규칙

- 컴포넌트 생성시 이름은 항상 대문자로 시작한다.

```jsx
export default function App() {}
```

- 화살표현식, 함수선언식 둘다 사용 가능하다.

```jsx
export default function App() {}
```

```jsx
const App = () => {};

export default App; //함수 표현식으로 사용시 해당 방법으로 내보내야 한다.
```

<br/>

### JSX

- Javascript + XML

#### 반드시 최상위 루트 태그가 존재해야 한다

```jsx
export default function App() {
  return (
    <>
      <div>1</div>
      <div>2</div>
    </>
  );
}
```

> <></> === <React.Fragment></React.Fragment> <br/> 리액트에서 제공하는 Fragment

#### 여러줄의 코드를 return 할 경우 `()` 소괄호 사용

```jsx
export default function App() {
  return (
    // 👈🏻
    <>
      <div>리액트</div>
      <div>리액트2</div>
    </>
  );
}
```

#### 받드시 태그는 닫아준다

```jsx
import Button from './components/Button';

export default function App() {
  return (
    <>
      <Button />
      <Button></Button>
    </>
  );
}
```

#### 표현식은 `{}` 사용한다

- 객체,배열,Boolean 중괄호를 사용하는 방식 불가능

```jsx
export default function Button() {
  const text = '버튼입니다!';
  return <div>{text}</div>;
}
```

<br/>

### Props의 활용법

#### 1. 매개변수 구조분해할당

- ⭐️ 가장 많이 사용하는 방식

```jsx
export default function PropsButton({
  type,
  name,
  disabled,
  children,
}: TButtonProps) {
  return (
    <button type={type} name={name} disabled={disabled}>
      {children}
    </button>
  );
}
```

#### 2. 컴포넌트 내에서 props의 구조분해할당

```jsx
export default function PropsButton(props: TButtonProps) {
  const { type, disabled, children } = props;
  return (
    <button type={type} disabled={disabled}>
      {children}
    </button>
  );
}
```

#### 3.나머지 매개변수를 활용한 구조분해할당

```jsx
type TButtonProps = React.ComponentProps<'button'> & {
  children: ReactNode,
};

export default function PropsButton(props: TButtonProps) {
  const { children, ...restButtonProps } = props;
  return <button {...restButtonProps}>{children}</button>;
}
```
