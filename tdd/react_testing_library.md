# React Testing Library

## 학습 키워드

- React Testing Library
- given - when - then 패턴
- Mocking
- Test fixture

<br/>

## React Testing Library

### 📖 React Testing Library

- Facebook에서 공식적으로 사용을 권장 (→ React로 서비스를 만든다면!)
- Behavior Driven Test(행위 주도 테스트) 방법론이 대두 되면서 함께 주목 받기 시작한 __테스팅 라이브러리__
- React 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구
- DOM을 활용해 우리가 UI를 직접 테스트할 수 있도록 해주는 라이브러리

> 📌 행위 주도 테스트(행위 주도 테스트)는 기존에 관행이던 Implementation Driven Test(구현 주도 테스트)의 단점을 보완하기 위한 방법론이며 사용자가 애플리케이션을 이용하는 관점에서 사용자의 실제 경험 위주로 테스트를 작성한다.

```html
<h2 class="title">제목</h2>
```

- __구현 주도 테스트 관점__
  - `h2`라는 태그가 쓰였고, `title` 이라는 클래스가 사용되었는지 여부를 테스트

- __행위 주도 테스트 관점__
  - 브라우저 화면에 `제목` 이라는 텍스트가 보일 뿐. 따라서, 사용자에게 어떤 컨텐츠가 현재 보이고, 사용자가 어떤 이벤트를 발생시켰을 때, 그에 따라 화면에 변화가 일어나는지를 테스트

<br/>

### ✍🏻 React Testing Library 정리

__⇒ 사용자 관점에서 브라우저(화면에) 일어나는 행위에 대해 테스트 할 수 있게 해주는 라이브러리__

<br/>

## given - when - then 패턴

### 📖 given - when - then 패턴

- __[준비] - [실행] - [검증] 단계로 테스트 코드를 나눠서 작성한다.__
  - Given [준비] : 시나리오 진행에 필요한 값을 설정, 테스트의 상태를 설정
  - When [실행] : 시나리오 진행 필요조건 명시, 테스트하고자 하는 행동
  - Then [검증] : 시나리오를 완료했을 때 보장해야 하는 결과를 명시, 예상되는 변화 설명

#### 예제를 통해 given - when - then 패턴으로 테스트 코드를 작성

```tsx
// TextField.tsx

import React, { useRef } from 'react';

type TextFiledProps = {
  label: string;
  placeholder: string;
  text: string;
  setText: (value: string) => void;
};

export default function TextField({
  label, placeholder, text, setText,
}: TextFiledProps) {
  const id = useRef(`input-${Math.random()}`);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { value } = event.target;
    setText(value);
  };

  return (
    <div>
      <label htmlFor={id.current}>
        {label}
      </label>
      <input
        id={id.current}
        type="text"
        placeholder={placeholder}
        value={text}
        onChange={handleChange}
      />
    </div>
  );
}
```

```tsx
// TextField.test.tsx
import { fireEvent, render, screen } from '@testing-library/react';

import TextField from './TextField';

const context = describe;
describe('TextField', () => {

  // 1. Given 
  // labe, text, setTest 함수,렌더링되어야 하는 컴포넌트 함수 준비 
  const label = '검색';
  const text = 'Search Text';
  const setText = jest.fn(); // → mocking

  beforeEach(() => {
    setText.mockClear();
    // 또는 jest.clearAllMocks();
  });

  function renderTextField() {
    render(
      <TextField
        label={label}
        placeholder="Input your name"
        text={text}
        setText={setText}
      />
    );
  }

  it('renders an input control', () => {
    // 2. When
    // TextField 컴포넌트가 화면에 렌더링 될 때
    renderTextField();


    // 3.Then 
    // 화면 Dom에서 input의 label, value, placeholder 확인
    screen.getByLabelText(label); 
    screen.getByDisplayValue(text);
    screen.getByPlaceholderText('식당이름');
  });

});
```

<br/>

## Mocking

- 테스트하고자 하는 코드가 의존하는 function이나 class에 대해 모조품을 만들어 '일단' 돌아가게 하는 것

__⇒ 단위 테스트를 작성할 때, 해당 코드가 의존하는 부분을 가짜(mock)로 대체하는 기법__

### 🤔 왜 가짜로 대체하는가?
>
> 테스트 하고 싶은 기능이 다른 기능들과 엮어있을 경우(의존) 정확한 테스트가 힘들기 때문에

<br/>

## Test fixture

> 📖 Fixture는 '고정되어 있는 물체'를 의미한다.

#### 🤔 Junit 팀에서 말하는 테스트 픽스처란?

- 테스트 실행을 위해 베이스라인으로서 사용되는 객체들의 __고정된 상태__

#### ✍🏻 내가 생각하는 테스트 픽스처는

- mocking으로 만들 객체들을 한곳에 모아두고 다르곳에서 재사용 하기 위한 용도

### 🔗 참고

- [React Testing Library 사용법](https://www.daleseo.com/react-testing-library/)
- [Web: React Testing Library의 개념과 간단한 예시](https://medium.com/hcleedev/web-react-testing-library의-개념과-간단한-예시-b94ea633bdaa)
- [모킹 Mocking 정리](https://inpa.tistory.com/entry/JEST-%F0%9F%93%9A-%EB%AA%A8%ED%82%B9-mocking-jestfn-jestspyOn)
- [Mocking으로 생산성까지 챙기는 FE 개발](https://tech.kakao.com/2021/09/29/mocking-fe/)
- [Unit Test에 나오는 Fixture와 Mock은 무엇일까?](https://zorba91.tistory.com/304)
- [메가테라 참고 GitBook - Test fixture](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week5/reacttestinglibrary#id-4.-mocking)
