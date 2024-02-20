# State

## 학습 키워드

- state (상태)
  - 소프트웨어 개발 3대 원칙 - DRY 원칙
- Lifting State Up
  - Inverse Data Flow
  - SSOT(Single Source of Truth)
- useState
  - 1급 객체(first-class object)란?

<br/>

## State

### 📖 State(상태)란?

- 컴포넌트 내부에서 `변경`되는 동적인 값
- 컴포넌트 렌더링에 영향을 주는 객체
- 컴포넌트의 내부에서 변경 가능한 데이터를 관리 해야 할 때 사용한다.

#### 🔄 컴포넌트가 Re-render 하는 조건

- 상태값이 변경 되었을 때 ( useState, Redux 등을 통한 상태값 변경시)
- Props의 값이 변경 되었을 떄
- 상위 컴포넌트의 state 변경되어 렌더링 하게되면 하위 컴포넌트도 다시 렌더링하게 된다.  

#### State가 되기 위한 조건

> 데이터라고 해서 모두 state 아니다!

1. 변경되지 않는 건 state로 다룰 가치가 없다. ⇒ 변경되는 값이어야 한다.
2. 부모 컴포넌트가 props를 통해 전달한다면 state가 아니다.
3. 다른 state나 props를 이용해 계산이 가능하다면 state가 아니다.

<br/>

#### 🌎 소프트웨어 개발 3대 원칙

- [DRY (Don't Repeat Yourself)](https://en.wikipedia.org/wiki/Don't_repeat_yourself) : 중복 배제

  - 같은 코드를 중복해서 작성하지 않는다.
  - 중복 코드를 최대한 줄여야 복잡한 시스템에서 개발 및 유지보수비용이 절감이 된다.  

- [KISS (Keep It Simple, Stupid)](https://en.wikipedia.org/wiki/KISS_principle) : 단순화

  - 단일 책임 원칙(SRP) , 인터페이스 분리 원칙
  - 간단한 코드가 가독성이 좋고, 버그를 유발하는것도 줄어든다.

- [YAGNI (You Ain't Gonna Need It)](https://en.wikipedia.org/wiki/KISS_principle) : 불필요하게 확장을 고려한 개발은 하지 말아라.

  - 불필요하게 확장에 치충한 코드가 생기거나 지금 당장 필요로 않은 로직을 만들지 말자는 원칙

<br/>

### ⬆ Lifting State Up

 🤔 state를 누가 관리하고, 소유해야 할까?

state가 하나의 컴포넌트에만 영향을 준다면 해당 컴포넌트에만 위치해도 된다. 하지만 두개의 컴포넌트가 하나의 state로 부터 영향을 받는다면 두 컴포넌트 __공통 상위 컴포넌트에 state를 끌어올려__ 공유해야한다. 이와 같은 방법을  __Lifting State Up__ `state 끌어올리기`라고 한다.

#### 🤖 그렇다면 Lifting State Up은 __역방향 데이터 흐름이 아닌가?__

단방향 데이터 흐름이라는 원칙에 따라 하위 컴포넌트는 상위 컴포넌트로 부터 전달받은 데이터의 형태 혹인 타입이 무엇인지만 알수 있고 데이터가 state, 하드코딩으로 입력된 내용인지도 알지 못한다. 그러므로 하위 컴포넌트에서 어떤 이벤트로 인해 상위 컴포넌트의 state가 바뀌는 것은 마치 역방향 데이터 흐름이라고 생각 된다.

> 상위 컴포넌의 "상태를 변경하는 함수" 그 자체를 하위 컴포넌트로 전달하고, 이 함수를 하위 컴포넌트가 실행한다.

⇒ `state 끌어올리기`는 state를 직접 전달하는 것이 아닌 __state 갱신 함수를__ 전달 받아 해당 함수를 실행시켜 state 변경사항을 반영한다.

#### 📖 [SSOT(Single Source of Truth)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%A7%84%EC%8B%A4_%EA%B3%B5%EA%B8%89%EC%9B%90)

- 단일 진실 공급원
- 모든 비즈니스 데이터를 하나의 공간에 저장하는 것을 말한다.

> React에서는 모든 state가 한곳에 있다는 뜻이 아니라, 각 state 마다 해당 정보를 소유하는 특정 컴포넌트가 있다는 의미 <br/>
컴포넌트마다 공유하는 state들을 각자 생성하는 것이 아닌, __부모컴포넌트(단일책임)에서 state를 생성해, 자식에게 전달하는것을 의미__

<br/>

### 🪝 useState

- React Hook이며 상태를 관리하기 위한 용도로 사용
- setState함수를 이용해서 state값을 변경하면 해당 컴포넌트는 화면에 다시 렌더링이 된다.
- 상태를 변화시는 setState 함수는 __비동기적으로__ 동작한다.

  ```jsx
  export default function Index() {
    const [state, setState] = useState(0);
    const onClick = () => {
      setCount(count + 1) // 0 + 1
      setCount(count + 1) // 0 + 1
      setCount(count + 1) // 0 + 1   <- 이 코드만 적용됨
      console.log(count) // 0
    };
    return (
      <div>
        <p> 현재 state : {state}</p>
        <button type="button" onClick={onClick}>
          +3
        </button>
      </div>
    );
  }
  ```

  위의 코드를 실행 보면 `console.log(state)` 값은 이전의 상태값인 0이 출력되고, state 값은 1이다.<br>
  ⇒ 이렇게 동작하는 이유는 setState 함수가 비동기적으로 동작하고 __React가 하나의 이벤트 핸들러 함수 내의 로직을 모두 읽을 때까지 기다린 다음에 일괄 처리(Batch)해 한번에 렌더링하기 때문이다.__

  🤔 __왜 React는 상태 값을 비동기적으로 처리하게 만들었을까?__
  > React 애플리케이션은 수 많은 컴포넌트와 상태값으로 이루어져있다. 이런 상황에서 단 하나의 상태가 변화할 때마다 관련된 뷰를 매번 리렌더링하는 것은 비효율과 함께 성능상의 문제를 야기한다.

  🤔 __연속된 setState를 처리하는 방법은?__
  - setState함수의 인자로 함수를 전달
  - useEffect 사용해서 setState 함수 다음에 실행 되어야 하는 코드를 동기적으로 실행되도록 처리 해줘야 한다.

<br/>

#### 📖 1급 객체(first-class object)란?

- 1급 시민의 조건을 충족하는 객체(Object)
- Javascript 에서는 객체는 1급 시민을 충족하기 때문에 1급객체 라고 부른다.

> 함수지향 언어가 되기 위해서는 1급 객체 즉, 1급 함수 개념을 만족해야되며, JS에서는 이를 보장하기 때문에 함수형 패러다임을 갖춘 언어라 볼 수 있다.

⇒ props로 함수를 넘겨 줄 수 있는건 1급 객체이기에 가능하다. 그래서 어떤 함수를 다른 함수에 인자로 넘겨주거나, 어떤 함수를 리턴값으로 사용할 수 있는 것이라고 이해했다.

<br/>

### 🔗 참고

- [소프트웨어 개발의 3개의 KEY 원칙 : KISS,YAGNI,DRY](https://hongjinhyeon.tistory.com/136)
- [Sharing State Between Components(컴포넌트 간의 state 공유)](https://hoonding.medium.com/react-공식문서-managing-state-sharing-state-between-components-컴포넌트-간의-state-공유-f5b7f8639203)
- [데이터흐름& state끌어올리기](https://doyu-l.tistory.com/303)
- [비동기로 동작하는 setState와 Batch](https://leo-xee.github.io/React/react-setstate/)
- [왜 state를 직접 변경하지 않고, useState를 사용해야 하나요?](https://velog.io/@daydreamplace/React-왜.-state를.-직접-변경하지-않고.-setState를.-사용하나요)
- [Javascript 1급 객체](https://0xd00d00.github.io/2021/11/27/js_first_object.html)
- [Javascript에서 왜 함수가 1급 객체일까요?](https://soeunlee.medium.com/javascript에서-왜-함수가-1급-객체일까요-cc6bd2a9ecac)
