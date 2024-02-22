# useRef

## 학습 키워드

- useRef

<br/>

## 📖 [useRef](https://react.dev/reference/react/useRef)

- 렌더링에 필요하지 않는 값을 참조 할 수 있는 React hook

> useRef는 .current 프로퍼티로 전달된 인자(initialValue)로 초기화된 변경 가능한 ref 객체를 반환합니다. 반환된 객체는 컴포넌트의 전 생애주기를 통해 유지될 것입니다.

상태(state)가 변경되면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하지만, 레퍼런스 객체의 현재 값(current)이 바뀌더라도 렌더링에 영향을 주지 않는다.

### 🤔 useRef 언제 사용하는가?

- 유지되는 값이 필요한 경우(컴포넌트 안의 변수 만들기)(feat.리렌더링 방지)
  - ex. Input id 관리 (UI에 관여하지 않는 값)
  - 실무 : 검색 후 이전의 키워드로 검색한 결과가 유지 되어야 하는 경우
- DOM 요소에 접근해야 하는 경우
  - input의 값을 입력하지 않았을 경우 focus(유효성체크)

#### 🤖 화면이 렌더링 되어도 유지되는 값이 필요한 경우

리액트의 state와 props은 값이 변경 될 때 마다 리렌더링이 일어나게 된다.
만약 값은 변경되지만 화면의 리렌더링이 일어나지 않게 하려면?
또한 화면이 리렌더링 되어도 값이 유지 되도록 하고 싶을 경우

```jsx
import { useRef, useState } from 'react';

export default function UseRef() {
  const [stateCount, setStateCount] = useState(0);
  const refCount = useRef(0);
  let count = 0;

  const handleStateUp = () => {
    setStateCount((stateCount) => stateCount + 1);
  };

  const handleLetUp = () => {
    count++;
    console.log(`변수 count 확인 ${count}`);
  };

  const handleRefUp = () => {
    refCount.current++;
    console.log(`ref count 확인 ${refCount.current}`);
  };

  return (
    <div>
      <p> state : {stateCount}</p>
      <p> 변수 : {count}</p>
      <p> ref : {refCount.current}</p>
      <button type="button" onClick={handleStateUp}>
        state up
      </button>
      <button type="button" onClick={handleLetUp}>
        변수 up
      </button>
      <button type="button" onClick={handleRefUp}>
        ref up
      </button>
    </div>
  );
}
```

> 자바스크립트 처럼 변수를 선언해서 사용하면 되지 않나?

컴포넌트내에서 자바스크립트 처럼 let을 이용해 변수를 선언해서 값을 수정하면 화면이 렌더링 되지 않는다. 그러나 만약 state 값이 변경되어 화면이 리렌더링 된다면 let을 이용해 선언한 변수는 다시 초기값으로 변경되어 값이 유지 되지 않는다.

#### 🤖 DOM 요소에 접근해야 하는 경우

JavaScript를 사용할 때는, 특정 DOM을 선택해야 하는 상황에 getElementById, querySelector같은 DOM Selector 함수를 사용해서 DOM을 선택한다. 하지만 리액트를 사용하면 DOM에 직접 접근할 일이 없다.
기본적으로 리액트가 render함수를 DOM을 업데이트 해준다.
하지만 아래의 경우처럼 DOM 요소에 접근 해야 하는 몇가지 경우도 있다.

- 스크롤 위치를 제어해야 할 때 (ex.Top 버튼)
- 특정 요소에 포커스를 맞춰야 할 때

<br/>

## 🤖 useRef 사용법

```jsx
import { useRef} from 'react';

const ref = useRef(initialValue); 
```

```jsx
import { useRef} from 'react';

export default function App(){

   const ref = useRef(null);
   console.log(ref);  // 출력 {current: null}
}
```

useRef는 처음에 제공한 초기값으로 설정된 단일 current 프로퍼티가 있는 ref 객체를 반환한다. ref 내부의 값을 변경하고자 한다면 `current`속성을 수정해야 한다.

#### useRef 대표적인 사용 예시

```jsx
import { useRef, useState } from 'react';

export default function UseRef() {

  const refCount = useRef(0);
  const inputEl = useRef(null);

  const handleRefUp = () => {
   refCount.current++;
   console.log(`ref count 확인 ${refCount.current}`);
  };

  const handleFocus = () => {
    inputEl.current.focus();
  };

  return (
    <div>
      <p> ref : {refCount.current}</p>
      <button type="button" onClick={handleRefUp}>
        ref up
      </button>
      <div>
          <input 
          type="text" 
          placeholder="검색어를 입력해주세요."
          ref={inputEl}
          />
          <button type="button" onClick={handleFocus}>
            검색
          </button>
      </div>
    </div>
  );
}
```

<br/>

### 🔗 참고

- [React 공식문서 스터디 useRef](https://react-ko.dev/reference/react/useRef)
- [useRef는 처음이라 :: 개념부터 활용 예시까지](https://overreacted.io/a-complete-guide-to-useeffect/)
- [useRef 훅 누구보다 쉽게 설명해드림 | 지금 까지 useState만 썼다면 꼭 봐라](https://youtu.be/kllWOdnU1Fg?si=pylKUSUYmhzpME4u)
