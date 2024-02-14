# 5. Props

## 학습 키워드

- Props
  - `순수 함수`
  - `Distructuring`
  - `Prop Drilling`

<br/>

## Props

### 📖 Props란?

- Properties의 줄임말
- 상위 컴포넌트가 하위 컴포넌트에 값(= Data)을 전달할 때 사용하는 속성이다.
- 부모 컴포넌트에서 전달하는 props 수정 가능하지만, 자식 컴포넌트는 props 읽기만 가능하다.

> 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 합니다. by.React 공식문서

#### 📖 순수 함수란?

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

#### 🤖 리액트는 컴포넌트를 왜 순수함수로 구성하려할까?

컴포넌트는 props와 state를 입력받고, React 엘리먼트를 반환한다.
이때 컴포넌트는 부작용(Side Effect)을 일으키지 않고, __동일한 props와 state를 입력으로 받으면 항상 동일한 엘리먼트를 반환해야한다.__

위와 같이 순수함수를 기반으로 함수형 컴포넌트를 작성하게 된다면 예측 가능성/테스트 용이성/성능 최적화/유지보수성 측면에서 효율적이다.

<br/>

### 🔗 참고

- [props를 왜 순수 함수처럼 사용해야할까?](https://tried.tistory.com/88)
- [순수 함수와 리액트의 관계](https://velog.io/@kcj_dev96/순수-함수와-리액트의-관계)
- [순수함수 / state와 props에 대해](https://junvelee.tistory.com/148)
