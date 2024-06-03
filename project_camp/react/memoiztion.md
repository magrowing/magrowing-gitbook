# Memoization

## 학습 키워드

- memoization
  - React.memo
  - useCallback
  - useMemo

<br/>

## [memoization](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)

> 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산값을 메모리에 저장함으로써 동일한 계산의 반복수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술

<br/>

### React.memo

- `컴포넌트`를 메모이제이션 할 때 사용한다.
- 컴포넌트의 prop가 변경되지 않는 경우 리렌더링을 하지 않는다.

```jsx
const MemoizedComponent = React.memo(SomeComponent, arePropsEqual?)
```

#### 👩🏻‍💻 예시

```jsx
import { useState } from 'react';

const Count = ({ count }: { count: number }) => {
  console.log('Count Rendered');
  return (
    <div>
      <h1>Count Component</h1>
      <h2>Count : {count}</h2>
    </div>
  );
};

export default function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');
  return (
    <div>
      <Count count={count} />
      <button type="button" onClick={() => setCount(count + 1)}>
        count 증가 버튼
      </button>
      <input
        type="text"
        placeholder="input 입력하면 어떻게 될까? "
        value={text}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setText(e.target.value)
        }
      />
    </div>
  );
}
```

해당 코드는 input 영역에 입력하게 되면 `Count` 컴포넌트가 불필요하게 리렌더링이 일어난다. `Count` 컴포넌트의 리렌더링이 일어나는 이유는 상위 컴포넌트 `App`의 text의 상태값이 업데이트 되면서 리렌더링이 일어나게 되는 것이다.

```jsx
import { useState, memo } from 'react';

const Count = ({ count }: { count: number }) => {
  console.log('Count Rendered');
  return (
    <div>
      <h1>Count Component</h1>
      <h2>Count : {count}</h2>
    </div>
  );
};

const MemoCount = memo(Count); // 👈🏻 메모이제이션한 컴포넌트

export default function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');
  return (
    <div>
      <MemoCount count={count} />
      <button type="button" onClick={() => setCount(count + 1)}>
        count 증가 버튼
      </button>
      <input
        type="text"
        placeholder="input 입력하면 어떻게 될까? "
        value={text}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setText(e.target.value)
        }
      />
    </div>
  );
}
```

`Count` 컴포넌트가 불필요하게 리렌더링을 막기 위해 `React.memo`를 활용해 컴포넌트를 메모이제이션 했으므로 props가 변경되지 않는 한 `Count` 컴포넌트는 리렌더링 발생하지 않는다.

<br/>

### useCallback

- `함수`를 메모이제이션 할 때 사용한다.
- 리렌더링 간에 함수의 정의를 캐싱해주는 React Hook

```jsx
const cachedFn = useCallback(() => {}, dependencies);
```

#### 👩🏻‍💻 예시

```jsx
import { memo, useState } from 'react';

const Count = ({ count }: { count: number }) => {
  console.log('Count Rendered');
  return (
    <div>
      <h1>Count Component</h1>
      <h2>Count : {count}</h2>
    </div>
  );
};

const Button = ({ onClick }: { onClick: () => void }) => {
  console.log('Button Rendered');
  return (
    <button type="button" onClick={onClick}>
      count 증가 버튼
    </button>
  );
};

const MemoCount = memo(Count);
const MemoButton = memo(Button);

export default function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');
  const onClickHandler = () => {
    setCount(count + 1);
  };
  return (
    <div>
      <MemoCount count={count} />
      <MemoButton onClick={onClickHandler} />
      <input
        type="text"
        placeholder="input 입력하면 어떻게 될까? "
        value={text}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setText(e.target.value)
        }
      />
    </div>
  );
}
```

해당 코드는 input 영역에 입력하게 되면 컴포넌트가 불필요하게 리렌더링을 막기 위해 memo를 사용했다. 하지만, `Button` 컴포넌트는 입력시 계속 리렌더링이 발생된다. 그이유는 무엇일까? `Button` 컴포넌트는 함수를 props로 받고 있다. 함수는 참조자료형으로 메모리의 주소값을 저장한다. 즉, 상위 컴포넌트 `App`의 리렌더링 하게 되면 `onClickHandler` 함수는 이전과 같은 함수가 아닌 새로운 함수로 생성되어 `Button` 컴포넌트가 리렌더링 되게 된다.

```jsx
const onClickHandler = useCallback(() => {
  setCount(count + 1);
}, []);
```

`onClickHandler` 함수를 `useCallback`을 사용해 메모이제이션 하게 되면 input 영역에 입력시 더이상 리렌더링이 되지 않는다.

#### 🚨 예상치 못한 이슈

불필요한 리렌더링은 없지만, `count 증가 버튼` 클릭시 count 값이 1로 한번만 업데이트되고, `Count` 컴포넌트가 더이상 업데이트 되지 않는다. 그이유는 무엇일까? `useCallback`의 의존성배열의 값은 빈값이여서 초기 렌더링시에만 해당 함수를 생성하고 그 이후에는 더이상 함수를 재생성하지 않는다. 이때 useCallback 인자로 전달받은 콜백함수에서 count 초기값 0을 참조하고 있다. `count 증가 버튼`를 클릭하면 `count` 상태값은 1로 변경되어 리렌더링이 발생된다. 다시 한번 `count 증가 버튼` 클릭하면 useCallback 인자로 전달받은 콜백함수의 count는 변경된 상태 1이 아닌 count 초기값 0 참조해서 계산해서 `setCount(count + 1)` 동작하면 1이기 때문에 `Count` 컴포넌트의 props는 변경되지 않았기에 리렌더링 발생하지 않고, `count` 상태값도 업데이트 되지 않는다.

```jsx
const onClickHandler = useCallback(() => {
  setCount((prev) => prev + 1); // 👈🏻 이전의 상태값을 참조하도록 변경
}, []);
```

초기 렌더링 시 count 초기값 상태를 참조하는게 아니라, 이전의 count의 상태값을 참조할 수 있는 콜백함수를 사용해서 setCount 함수가 업데이트 되도록 변경해야 `Button` 컴포넌트는 리렌더링 발생하게 된다.

<br/>

### useMemo

- `값`를 메모이제이션 할 때 사용한다.
- 리렌더링 사이에 계산 결과를 캐싱할 수 있게 해주는 React Hook
  - 비용이 높은 로직의 재계산 생략

```jsx
const cachedValue = useMemo(calculateValue, dependencies);
```

#### 👩🏻‍💻 예시

```jsx
import { useState } from 'react';

// 3000만개의 배열 데이터
const initialItems = new Array(29_999_999).fill(0).map((_, i) => {
  return {
    id: i,
    selected: i === 29_999_998,
  };
});

export default function App() {
  const [count, setCount] = useState(0);
  // 3000만개의 배열 데이터를 렌더링 마다 재생성하고 있음
  const [items] = useState(initialItems);
  const selectItems = items.find((item) => item.selected);
  return (
    <div>
      <h1>Count : {count}</h1>
      <button type="button" onClick={() => setCount((prev) => prev + 1)}>
        증가
      </button>
      <p>{selectItems?.id}</p>
    </div>
  );
}
```

해당코드는 `App` 컴포넌트가 리렌더링을 할 때 마다 3000만개의 배열 데이터를 재생성하고 있다. 증가 버튼을 클릭시 3000만개의 배열 데이터를 재생성하다보니 연산 비용이 크기때문에 count 상태 업데이트시 화면에 업데이트 값이 출력되지 않고 있다.

```jsx
const selectItems = useMemo(() => items.find((item) => item.selected), []);
```

`useMemo`를 통해 메모이제이션 한 값을 리렌더링시 재사용하기 때문에 화면 업데이트가 빠르게 동작하게 되었다.
