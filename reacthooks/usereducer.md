# useReducer

## 학습 키워드

- useReducer

<br/>

## 📖 [useReducer](https://react.dev/reference/react/useReducer)

- useReducer는 컴포넌트에 reducer를 추가할 수 있게 해주는 React Hook
- reducer를 사용하여 컴포넌트의 __상태 로직을 관리하는 데 사용된다.__
- 주로 useState보다 복잡한 상태 로직을 처리해야할 때 사용한다.
- 컴포넌트 외부에서 State 관리 로직 분리 가능

<br/>

## 🤖 useReducer 사용법

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

```jsx
import { useReducer } from 'react';

function reducer(state, action) {
  // ...
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
}
```

### Parameter

- `reducer 함수` : 상태가 어떻게 업데이트를 할지를 명시해둔 함수.
  - 순수함수
  - 인자로 state, Action을 전달하면 새로운 상테(state)값을 반환한다.
- `initialArg` : 초기값
  - 초기 상태를 계산하는 방법은 다음 init 인자에 따라 달라진다.
- `init(선택사항)` : 초기값을 반환하는 초기화 함수
  - 초기화 함수가 전달되지 않은 경우, 초기 값은 initialArg 값으로 설정된다.
  - 그렇지 않은 경우, 초기값은 init(initialArg)를 호출한 결과로 설정된다.

> reducer 함수는 반드시 불변성을 지켜 새로운 상태를 반환 해야한다.

```jsx
function reducer(state, action) {
  switch(action.type){
    case 'Action.Type' : {
      return {...state, name:'New Name'} // 스프레드 문법을 사용하여 기존 state의 불변성을 유지
    }
  }
}
```

### Returns

- useReducer는 두 값을 가지는 배열을 반환한다.
  - `state` : 첫 번째 렌더 중 init(initialArg) 또는 initialArg로 설정
  - `dispatch 함수` : 상태를 다른 값으로 업데이트하고 재렌더를 발생시키는 함수 (상태변화를 발동시키는 트리거 역활)
    - 인자로 Action 전달

> dispatch 함수 <br/> 상태를 다른 값으로 업데이트하고 재렌더를 발생시킨다. dispatch 함수의 유일한 인자로 액션을 전달한다.

```jsx
const [state, dispatch] = useReducer(reducer, { age: 42 });

function handleClick() {
  dispatch({ type: 'incremented_age' });
  // ...
```

<br/>

### 🔗 참고

- [useReducer를 어떻게, 그리고 언제 사용할까?](https://eun-jee.com/post/front-end/react/04-useReducer/)
- [변하지 않는 상태를 유지하는 방법, 불변성(Immutable)](https://evan-moon.github.io/2020/01/05/what-is-immutable/)
