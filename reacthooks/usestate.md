# useState

## 학습 키워드

- useState
  - initialState
  - state
  - setState
    - 비동기와 일괄처리(batch)
    - 클로져와 큐

<br/>

## 📖 [useState](https://react.dev/reference/react/useState#avoiding-recreating-the-initial-state)

- React hook
- state(상태)를 생성하고 업데이트(setState)를 통해 화면(UI)을 (리)렌더링하는 기능을 제공한다.

<br/>

## 🤖 useState 사용법

```jsx
import { useState } from "react"; // 사용을 위해 import 해줘야 하고,

const [state, setState] = useState(초기값); // 해당 코드처럼 최상위에 선언해준다.
```

state 생성과 동시에 가져야 할 초기값을 useState 함수에 인자로 넣어주면 state, setState 두가지의 요소를 배열 형태로 리턴해준다.

최초 렌더링시 state는 인자로 받은 초기값을 가지고 있고, setState는 state를 변경하는 함수를 가지고 있다.

### useState(initialState)

```jsx
import { useState } from 'react';

function MyComponent() {
  const [age, setAge] = useState(28);
  const [name, setName] = useState('Taylor');
  const [todos, setTodos] = useState(() => createTodos());

}
```

- 매개변수 initialState 은 상태의 초기값을 지정한다.
- 매개변수로 콜백함수도 선언이 가능하다.
  - 대규모 배열을 생성하거나 비용이 많이 드는 계산을 수행하는 경우
  - [useEffct 사용없이 useState 초기값으로 재생성 방지](https://velog.io/@hjthgus777/다시-한번-useState-를-파헤쳐보자)

### state

```jsx
const [form, setForm] = useState({
   firstName: 'Barbara',
   lastName: 'Hepworth',
   email: 'bhepworth@sculpture.com',
});

// 직접적으로 값을 입력하게 되면 리렌더링이 발생하지 않음.
form.firstName = "Taylor" 

// setForm 상태변화 함수를 통한 변경 
setForm({
  ...form,
  firstName: 'Taylor'
});
```

state를 변경하고 싶다면 setState 함수를 호출하고 인자로 변경하고 싶은 값을 넣어주면 state값이 변경되고 state를 가지고 있는 컴포넌트가 렌더링(업데이트)하게 된다.

### setState(nextState)

- 비동기로 동작하여 state를 변경해주는 함수이다.
- 이전의 상태값을 얻고 싶다면 콜백함수를 매개변수로 넣어주어야 한다.

```jsx
import { useState } from 'react';

export default function Index() {
  const [count, setCount] = useState(0);
  const onClick = () => {
    setCount(count + 1); // 0 + 1
    setCount(count + 1); // 0 + 1
    setCount(count + 1); // 0 + 1   <- 이 코드만 적용됨
    console.log(count); // 0
  };
  return (
    <div>
      <p> 현재 state : {count}</p>
      <button type="button" onClick={onClick}>
        +3
      </button>
    </div>
  );
}
```

  위의 코드를 실행 보면 `console.log(state)` 값은 이전의 상태값인 0이 출력되고, state 값은 1이다. <br>
  ⇒ 이렇게 동작하는 이유는 setState 함수가 비동기적으로 동작하고 __React가 하나의 이벤트 핸들러 함수 내의 로직을 모두 읽을 때까지 기다린 다음에 일괄 처리(Batch)해 한번에 렌더링하기 때문이다.__

#### 🤔 왜 React는 상태 값을 비동기적으로 처리하게 만들었을까?

  > React 애플리케이션은 수 많은 컴포넌트와 상태값으로 이루어져있다. 이런 상황에서 단 하나의 상태가 변화할 때마다 관련된 뷰를 매번 리렌더링하는 것은 비효율과 함께 성능상의 문제를 야기한다.

#### 🤔 연속된 setState를 처리하는 방법은?

- setState함수의 인자로 함수를 전달한다.
- useEffect의 의존성 배열을 활용하면 된다.

```jsx
import { useState } from 'react';

export default function Index() {
  const [count, setCount] = useState(0);
  const onClick = () => {
    setCount((count) => count + 1); // 0+1 count → 1
    setCount((count) => count + 1); // 1+1 count → 2
    setCount((count) => count + 1); // 2+1 count → 3
    console.log(count); // 0
  };
  return (
    <div>
      <p> 현재 state : {count}</p>
      <button type="button" onClick={onClick}>
        +3
      </button>
    </div>
  );
}

```

<br/>

### 🔗 참고

- [다시 한번 useState를 파헤쳐보자.](https://velog.io/@hjthgus777/다시-한번-useState-를-파헤쳐보자)
- [비동기로 동작하는 setState와 Batch](https://leo-xee.github.io/React/react-setstate/)
- [useState는 동기 비동기? (동기적 처리)](https://velog.io/@alstnsrl98/useState는-동기-비동기-동기적-처리)
- [useState 의 setState는 비동적으로 동작](https://velog.io/@jhplus13/위스타그램-개발노트React)
- [useState와 useEffect 좀 더 깊게 알아보기](https://gml9812.github.io/frontend/useState-and-useEffect/)
