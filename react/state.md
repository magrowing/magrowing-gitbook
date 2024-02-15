# 6. State

## 학습 키워드

- state (상태)
  - 소프트웨어 개발 3대 원칙 - DRY 원칙
- Lifting State Up
  - SSOT(Single Source of Truth)
- useState
  - 1급 객체(first-class object)란?

<br/>

## State

### 📖 State(상태)란?

- 컴포넌트 내부에서 `변경`되는 동적인 값
- 컴포넌트 렌더링에 영향을 주는 객체
- 컴포넌트의 내부에서 변경 가능한 데이터를 관리 해야 할 때 사용한다.

#### 컴포넌트가 Re-render 하는 조건

- 상태값이 변경 되었을 때 ( useState, Redux 등을 통한 상태값 변경시)
- Props의 값이 변경 되었을 떄
- 상위 컴포넌트의 state 변경되어 렌더링 하게되면 하위 컴포넌트도 다시 렌더링하게 된다.  

#### State가 되기 위한 조건

> 데이터라고 해서 모두 state 아니다!

1. 변경되지 않는 건 state로 다룰 가치가 없다. ⇒ 변경되는 값이어야 한다.
2. 부모 컴포넌트가 props를 통해 전달한다면 state가 아니다. ⇒ props로 전달되는 값은 state가 아니다.
3. 다른 state나 props를 이용해 계산이 가능하다면 state가 아니다.

<br/>

#### 🌎 소프트웨어 개발 3대 원칙

- [DRY (Don't Repeat Yourself)](https://en.wikipedia.org/wiki/Don't_repeat_yourself) : 중복 배제

  - 같은 코드를 중복해서 작성하지 않는다.
  - 중복 코드를 최대한 줄여야 복잡한 시스템에서 개발 및 유지보수비용이 절감이 된다.  

- [KISS (Keep It Simple, Stupid)](https://en.wikipedia.org/wiki/KISS_principle) : 단순화

  - 단일 책임 원칙(SRP) , 인터페이스 분리 원칙
  - 간단한 코드가 가독성이 좋고, 버그를 유발하는것도 줄어든다.

- [YAGNI (You Ain't Gonna Need It)](https://en.wikipedia.org/wiki/KISS_principle) : 불필요하게 확장을 고려한 개발은 하지 말아라

  - 불피룡하게 확장에 치충한 코드가 생기거나 지금 당장 필요로 않은 로직을 만들지 말자는 원칙

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

### 🔗 참고

- [소프트웨어 개발의 3개의 KEY 원칙 : KISS,YAGNI,DRY](https://hongjinhyeon.tistory.com/136)
- [Sharing State Between Components(컴포넌트 간의 state 공유)](https://hoonding.medium.com/react-공식문서-managing-state-sharing-state-between-components-컴포넌트-간의-state-공유-f5b7f8639203)
- [데이터흐름& state끌어올리기](https://doyu-l.tistory.com/303)
- [왜 state를 직접 변경하지 않고, useState를 사용해야 하나요?](https://velog.io/@daydreamplace/React-왜.-state를.-직접-변경하지-않고.-setState를.-사용하나요)
