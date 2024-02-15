# 5. Props

## 학습 키워드

- Props
  - `순수 함수`
  - 구조 분해 할당 `Distructuring`
  - `Prop Drilling`

<br/>

## Props

### 📖 Props란?

- Properties의 줄임말
- 데이터를 전달해주는 객체
- 상위 컴포넌트가 하위 컴포넌트에 값(= Data)을 전달할 때 사용하는 속성이다.
- 부모 컴포넌트에서 전달하는 props 수정 가능하지만, 자식 컴포넌트는 props 읽기만 가능하다.

> 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 합니다. by.React 공식문서

#### 순수 함수란?

- 순수 함수는 함수형 프로그래밍에서 쓰이는 개념이다.
- 함수 내부에서 사용되는 값들이 __외부의 영향을 주지 않고__ 어떠한 __Input을 전달해도 똑같은 상태(타입)로 output을 반환해주는 기능을__ 하는 함수
__⇒ 입력값에 대해 항상 동일한 출력값을 반환하는 함수로, 외부상태에 영향을 끼치는 사이드 이펙트가 없는 함수를 의미한다.__

```javascript
// 순수 함수 예시 
function add(a, b) {
  return a + b;
}
```

```javascript
//  순수 함수 아닌 예시
function withdraw(a, b){
  a.num -= b;
  // 입력 받은 인자에 대입연산자를 사용해서 입력받은 값이 변경 되었다.
}
```

#### 🤖 리액트는 컴포넌트를 props를 다룰 때 순수 함수처럼 구성해야 할까?

- __예측 가능성__<br>
  동일한 props를 전달 했을 때 동일한 결과를 반환하기 때문에 예측 가능한 동작을 보장 할 수 있다.
- __테스트 용이성__<br>
  입력한 값에 따라 항상 동일한 출력값을 반환하기에 Unit Test(단위 테스트)하기가 쉽다.
- __성능 최적화__<br>
  상위 컴포넌트에서 전달 받은 하위 컴포넌트에서 props는 읽기 전용이므로 변경 할 수 없다.<br/>
  props가 변경되지 않은 경우 렌더링을 건너뛸 수 있으므로 성능을 최적화 할 수 있다.
- __유지보수성__<br>
  부작용이 없는 순수함수는 코드의 의도를 명확하게 표현 할 수 있어 유지보수가 쉽다.

> ⇒ 컴포넌트가 순수 함수로 작성되면, 상태변화를 추적하기 쉽고, 코드의 복잡성을 줄일 수 있다.<br/>
따라서 리액트에서는 순수 함수를 기반으로 하는 함수형 컴포넌트를 권장하고 있다.

#### 🤖 React는 Props를 Date Destructuring을 통해 사용하기를 권장하고 있다

```jsx
// 상위 컴포넌트 
const App = () => {
  return (
    <>
      <PropsNotSeparated name="홍길동" age={30} />
      <PropsSeparated name="홍길동" age={30} />
    </>
  );
};

// 하위 컴포넌트 
function PropsNotSeparated(props){
  return (
    <p>{props.name}</p>
    <p>{props.age}</p>
  );
}

function PropsSeparated({name,age}){
  return (
    <p>{name}</p>
    <p>{age}</p>
  );
}
```

- Data destructuring(구조분해할당)을 통해 전달받은 props의 내용들을 직관적이고 가독성 좋은 코드가 될 수 있고, 하나가 아닌 여러 props를 전달 받는 경우 효과적으로 활용 할 수 있다.

<br/>

### 🤔 Prop Drilling 이란?

Props는 상위 컴포넌트에서 하위컴포넌트로 데이터를 전달하고 전달 받은 하위 컴포넌트는 props 값을 변경 할 수 없기에 데이터는 단방향 흐름을 가지게 된다. `Prop Driling`은 Props를 통해 데이터를 전달하는 과정에서 중간 컴포넌트는 데이터가 필요하지 않음에도 자식 컴포넌트에 데이터 전달하기 위해 props를 전달해야 하는 과정을 의미한다.

#### Prop Drilling 문제점

컴포넌트의 계층구조가 복잡해지고 많아지게 되는 경우 props로 데이터를 전달 받게 된다면 비효율적인 현상들이 발생하게 된다.

- 사용하지도 않는 props 전달로 인해 코드가 길어짐
- props의 이름이 변경 된다면

#### Prop Drilling 해결 방법

- 전역 상태관리 라이브러리 사용
  - redux, Mobx, recoil 라이브러리를 사용하여 값이 필요한 컴포넌트에 직접 불러서 사용
- Children을 적극적으로 사용

<br/>

### ✍🏻 Props 정리

- 상위 컴포넌트에서 하위 컴포넌트에 값을(데이터)를 전달하기 위한 속성이며, 데이터 전달하기 위한 객체이다.
- 하위 컴포넌트는 전달받은 props 값을 변경 할 수 없으며, 구조분해할당을 통해 props 객체를 받아 사용한다.
- 상위 컴포넌트에서 하위 컴포넌트로 props를 전달하고, 전달 받은 props 값을 변경 할 수 없어 단방향 흐름을 갖게 되며, 컴포넌트의 계층구조가 복잡해지고 많아지게 된다면 props로 데이터를 전달받는 과정에서 비효율적인 현상들이 발생하게 된다. `Prop Drilling`

<br/>

### 🔗 참고

- [Props란? 컴포넌트에 데이터 전달하는 프로퍼티](https://life-with-coding.tistory.com/509)
- [props를 왜 순수 함수처럼 사용해야할까?](https://tried.tistory.com/88)
- [순수 함수와 리액트의 관계](https://velog.io/@kcj_dev96/순수-함수와-리액트의-관계)
- [순수함수 / state와 props에 대해](https://junvelee.tistory.com/148)
- [순수함수. 불변성과 사이드이펙트 /리액트의 state와 props](https://jellajellaangela.tistory.com/79)
- [React - props와 data destructuring](https://velog.io/@hyeseong/React-props-data-destructuring)
- [상태관리와 Props Drilling](https://velog.io/@ahsy92/기술면접-상태관리와-Props-Drilling)
