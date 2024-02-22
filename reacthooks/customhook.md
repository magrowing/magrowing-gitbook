# Custom Hook

## 학습 키워드

- Custom Hook
  - Hook의 규칙
- usehooks-ts
  - useBoolean
  - useCounter
  - useEffectOnce
  - useFetch
    - SWR
    - React-Query
  - useInterval
  - useEventListener
  - useLocalStorage
  - useDarkMode

<br/>

## 📖 [Custom Hook](https://react-ko.dev/reference/react/useRef)

- __컴포넌트 로직을 함수로 뽑아내어 재사용할 수 있는 방법__
- React의 State와 Lifecycle 관리 로직을 공통화 할 수 있도록 도와주는 역활 (컴포넌트간의 로직 공유)

### 🤖 Custom hook 사용법

- `use`로 시작해야 하고 PascalCase로 이름을 생성한다.
- 📁 `hooks`폴더를 생성하고 하나의 파일로 분리 후 사용

### 📌 [Hook](https://ko.legacy.reactjs.org/docs/hooks-rules.html)의 규칙

#### 1. 최상위(at the Top Level)에서만 Hook을 호출해야 한다

- 반복문, 조건문 혹은 중첩된 함수 내에서 Hook을 호출하지 않는다.

```jsx
// 처음에는 콜백 함수나 조건문 안에서 Hook을 호출하는 실수를 하게 된다.
// Bad!!
if (playing) {
 const products = useFetchProducts();
 console.log(products);
}
```

#### 2. React 함수 내에서 Hook을 호출해야 한다

- React 함수 컴포넌트에서 Hook을 호출
- Custom Hook에서 Hook을 호출

<br/>

## 📖 [usehooks-ts](https://usehooks-ts.com/)란?

- Hook 라이브러리
- Hook을 어떻게 만들었는지 코드를 볼 수 있고 Custom Hook을 만드는데 영감을 얻을 수 있다.

### usehooks-ts 설치

```shell
npm i usehooks-ts
```

### ⚙️ useBoolean

- [useBoolean](https://usehooks-ts.com/react-hook/use-boolean)
- 참/거짓을 다룰 땐 toggle 같이 의도가 명확한 함수를 쓰는 게 좋다.

```tsx
import { useState } from 'react';
import { useBoolean } from 'usehooks-ts';

export default function Toggle() {

  const [stateToggle, setStateToggle] = useState(false);
  const { value: myToggle, toggle: myToggleFun } = useBoolean(false); 
  // setStateToggle(!stateToggle) 보다 toggle 혹은 myToggleFun이 의도가 명확하다.

  const handleState = () => {
    setStateToggle(!stateToggle); 
  };

  return (
    <div>
      <p> useState : {stateToggle ? 'True' : 'False'}</p>
      <p> useBoolean : {myToggle ? 'True' : 'False'}</p>
      <button type="button" onClick={handleState}>
        useState hook 토글 버튼
      </button>
      <button type="button" onClick={myToggleFun}>
        useBoolean hook 토글 버튼
      </button>
    </div>
  );
}
```

### ⚙️ useCounter

- [useCounter](https://usehooks-ts.com/react-hook/use-counter)

```tsx
import { useCounter } from 'usehooks-ts'; // 사용해보니,훨씬 코드의 의도가 명확해지고 간결하다. 

export default function Toggle() {
  const { count, increment } = useCounter(0);

  return (
    <div>
      <p> useCounter : {count}</p>
      <button type="button" onClick={increment}> 
        useCounter hook 버튼
      </button>
    </div>
  );
}
```

### ⚙️ useEffectOnce

- [useEffectOnce](https://usehooks-ts.com/react-hook/use-effect-once)
- 의존성 배열을 빈 배열로 넣어서 한 번만 실행하는 Effect를 사용하는 경우가 많은데 useEffectOnce
hook 이름에서부터 의도가 명확히 드러난다.

```jsx
const [products, setProducts] = useState<Product[]>([]);

useEffectOnce(() => {
  const fetchProducts = async () => {
  const url = 'http://localhost:3000/products';
  const response = await fetch(url);
  const data = await response.json();
  setProducts(data.products);
 };

 fetchProducts();
});

return products;
```

### ⚙️ useFetch

- [useFetch](https://usehooks-ts.com/react-hook/use-fecth)
- 정말 간단히 쓸 때 좋음, 그러나 기능이 많지 않다.

```jsx
export default function useFetchProducts() {
  const url = 'http://localhost:3000/products';
  const { data } = useFetch(url);
  if (!data) { 
    return []; 
  }
  return data.products;
}
```

- 몇 가지 기능이 살짝 더 있는 __useFetch 라이브러리가__ 따로 있다. → [use-http](https://use-http.com) (많이 사용하진 않는다.)
- 사용법이 더 복잡헤도 괜찮다면, 캐시 이슈를 고려하는 다른 대안의 라이브러리가 있다.
  - [SWR](https://swr.vercel.app/ko) : 데이터 가져오기를 위한 React Hooks
  - [React Query](https://tanstack.com/query/latest)

> SWR을 사용하면 컴포넌트는 주기적으로 데이터가 바뀌었는지 확인한다. 예를 들어, 게시판이면 게시물이 올라왔는지 확인한다. React Query는 이보다 더 복잡한 것을 다룰 때 사용한다.

### ⚙️ useInterval

- [useInterval](https://usehooks-ts.com/react-hook/use-interval)
- React에서 setInterval 등을 쓸 때는 주의해야 할 부분이 있어서 Custom Hook을 만들어서 해결해야 한다.
- __해당 Hook 쓰길 추천한다! setInterval 이슈가 분명이 있다.__
  - [useEffect관점](https://overreacted.io/ko/a-complete-guide-to-useeffect/) → Not Found(?)
  - [Ref 활용](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)
  - [코좀봐코 - React에서의 타이머 part 1 : setInterval 말고 이것!](https://youtu.be/2tUdyY5uBSw?si=6WEJ2BXMeAh7pWnI)
  - [코좀봐코 - React에서의 타이머 part 2 : useIntervalValue Hook 만들어서 카운트다운 쉽게 구현하기](https://youtu.be/zBmTqF0GpN4?si=jj9L9pKd5q9IqQuz)
  
### ⚙️ useEventListener

- [useEventListener](https://usehooks-ts.com/react-hook/use-event-listener)
- ⭐️ 강력 추천 ⭐️
- 모든 종류의 이벤트를 확인할 수 있다.
- 특히 __dispatchEvent로__ 전달되는 커스텀 이벤트에 반응하기 좋다.

### ⚙️ useLocalStorage

- [useLocalStorage](https://usehooks-ts.com/react-hook/use-local-storage)
- LocalStorage에 객체를 넣는 경우가 많다. 이와 같은 경우  stringfy 와 parse를 자동으로 해준다.
- __이벤트를 통해(dispatchEvent + useEventListener) 다른 컴포넌트와 동기화하는 게 매우 중요한 특징__

### ⚙️ useDarkMode

- [useDarkMode](https://usehooks-ts.com/react-hook/use-dark-mode)
- os 운영체제의 다크테마를 활용하고 싶을 때

<br/>

### ✍🏻 Custom Hook, usehooks-ts 정리

- `Custom Hook`을 활용한다면 __재사용__ 측면에서 리액트의 강점을 제대로 사용 할 수 있다. 재사용 할 수 있도록 로직을 잘 생각해야 할 것 같다.
- `usehooks-ts` 참고 할만 한 내용이 많고, 활용성 측면 야살님은 추천한다.
- 별도 설치 없이 코드를 복사해서 사용해도 좋고, Custom Hook의 활용 할 수 있는 방법이 많기 때문에 자주 보면서 영감을 얻을 수 있다.

<br/>

### 🔗 참고

- [React Custom hook을 어떻게 사용할까](https://medium.com/@asdfg9377/react-custom-hook에-대해-이해하고-사용하기-75f0e8f55df6)
