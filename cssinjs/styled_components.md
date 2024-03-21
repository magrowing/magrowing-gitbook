# Styled Components

## 학습 키워드

- Styled Components
  - 설치 및 설정
  - 기본 문법
  - props
  - attrs
  - Reset CSS
  - Global Style
  - Theme

<br/>

## 💅🏻 [styled-components](https://styled-components.com/)

- CSS in JS 라이브러리
- CSS의 문제점을 해결하고자 자바스크립트 코드로 CSS를 작성하는 방식으로 설계
- `Tagged Template Literals` 문법 사용

> 📖 [Tagged Template Literals](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals#tagged_templates) <br/>
ES6에 추가 된 문법으로 template literals 의 더욱 발전된 한 형태로 태그를 사용하면 템플릿 리터럴을 함수로 파싱 할 수 있다. 태그 함수의 첫번째 인수는 문자열 값의 배열을 포함하고, 나머지 인수는 표현식관 관련되어 있다.

<br/>

### ⚙️ 설치 및 설정

- 패키지 설치
  - [Babel Plugin](https://styled-components.com/docs/tooling#babel-plugin)을 SWC에서 쓸 수 있도록 포팅한 것도 함께 설치 [@swc/plugin-styled-components](https://github.com/swc-project/plugins/tree/main/packages/styled-components)
  - ⭐️ SSR 지원 등을 위한 공식 권장사항

```shell
npm i styled-components

npm i -D @types/styled-components @swc/plugin-styled-components
```

- vscode-styled-components Extention 설치
  - [vscode-styled-components](https://marketplace.visualstudio.com/items?itemName=styled-components.vscode-styled-components)

- `.swcrc` 파일 생성

```
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "displayName": true,
            "ssr": true
          }
        ]
      ]
    }
  }
}
```

<br/>

### 기본 사용법

- sytel.원하는 태그 쓰고 리터널방식으로 css 속성들을 넣어주면 된다.

```jsx
import React from 'react';
import styled from 'styled-components';

const Circle = styled.div`
  width: 5rem;
  height: 5rem;
  background: black;
  border-radius: 50%;
`;

function App() {
  return <Circle />;
}

export default App;
```

<br/>

### props

- [Passed props](https://styled-components.com/docs/basics#passed-props)
- props를 이용해서 활성화 여부를 표현하거나 특정 스타일을 잡아 주고 싶을 때 유용

```jsx
import { useState } from 'react';

import styled, { css } from 'styled-components';

type ButtonProps = {
  active?: boolean;
};

const Button = styled.button<ButtonProps>`
  width: 100px;
  height: 100px;
  background-color: #fff;
  color: #000;
  border: 1px solid gray;

  ${(props) =>
    props.active &&
    css`
      background-color: #00f;
      color: #fff;
      border: 1px solid #00f;
    `}
`;

export default function Switch() {
  const [toggle, setToggle] = useState(false);

  const handleClick = () => {
    setToggle(!toggle);
  };

  return (
    <Button onClick={handleClick} active={toggle}>
      ON/OFF
    </Button>
  );
}
```

<br/>

### attrs

- [Attaching additional props](https://styled-components.com/docs/basics#attaching-additional-props)
- 기본 속성을 추가할 수 있다. 불필요하게 반복되는 속성을 처리할 때 유용
  - 버튼을 만들 때 적극적으로 활용  

```jsx
import styled, { css } from 'styled-components';

type ButtonProps = {
  type?: 'button' | 'submit' | 'reset';
};

const Button = styled.button.attrs<ButtonProps>((props) => {
  return {
    type: props.type ?? 'button',
  };
})`
  width: 100px;
  height: 100px;
  background-color: #fff;
  color: #000;
  border: 1px solid gray;
`;

export default Button
```

<br/>

### Reset CSS

- [styled-reset](https://github.com/zacanger/styled-reset) 패키지 설치

> styled-components 에서 [Reset CSS](https://meyerweb.com/eric/tools/css/reset/) 처럼 태그들의 기본 속성을 초기화 하기 위해 설치

```shell
npm i styled-reset
```

- 최상위 컴포넌트 적용

```jsx
// App.tsx

import { Reset } from 'styled-reset';

export default function App() {
  return (
    <div>
      <Reset />
    </div>
  );
}
```

<br/>

### Global Style

- `styles` 폴더 , `GlobalStyle.ts` 파일 생성

```shell
mkdir styles
touch styles/GlobalStyle.ts
```

- [createGlobalStyle](https://styled-components.com/docs/api#createglobalstyle)  사용해서 전역 스타일 지정
  - [box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)
  - [The 62.5% Font Size Trick](https://www.aleksandrhovhannisyan.com/blog/62-5-percent-font-size-trick/)
  - [word-break](https://developer.mozilla.org/ko/docs/Web/CSS/word-break)

```ts
// GlobalStyle.ts

import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  html {
    box-sizing: border-box;
  }

  *,
  *::before,
  *::after {
    box-sizing: inherit;
  }

  html {
    font-size: 62.5%; 
  }

  body {
    font-size: 1.6rem;
  }

  :lang(ko) {
    h1, h2, h3 {
      word-break: keep-all;
    }
  }
`;

export default GlobalStyle
```

- 최상위 컴포넌트 적용

```jsx
import { Reset } from 'styled-reset';

import GlobalStyle from '../styles/GlobalStyle';

export default function App() {
  return (
    <div>
      <Reset />
      <GlobalStyle />
    </div>
  );
}
```

<br/>

### Theme

- [Theming](https://styled-components.com/docs/advanced#theming) → `<ThemeProvider>` 활용
- 디자인 시스템의 근간을 마련하는 활용
- ex.다크모드 대응하기 쉬움

#### styles/`defaultTheme.ts` 파일 생성

```ts
const defaultTheme = {
  colors: {
    background: '#FFF',
    text: '#000',
    primary: '#F00',
    secondary: '#00F',
  },
};

export default defaultTheme;
```

#### 역으로 `typeof` 를 사용해서 Theme 타입 지정 → styles/`Theme.ts` 파일 생성

```ts
import defaultTheme from './defaultTheme';

type Theme = typeof defaultTheme;

export default Theme;
```

#### styles/`darkTheme.ts` 파일 생성해서 Theme 으로 타입 지정

```ts
import Theme from './Theme';

const darkTheme : Theme = {
  colors: {
    background: '#000',
    text: '#FFF',
    primary: '#F00',
    secondary: '#00F',
  },
};

export default darkTheme;
```

#### 최상위 `<ThemeProvider>` 컴포넌트를 통해 공급

```jsx
import { Reset } from 'styled-reset';
import { ThemeProvider } from 'styled-components';

import GlobalStyle from '../styles/GlobalStyle';
import defaultTheme from '../styles/defaultTheme';

import Greeting from './components/Greeting';
import Switch from './components/Switch';

export default function App() {

  const theme = defaultTheme;

  return (
    <ThemeProvider theme={theme}> {/* 👈🏻 Theme 공급 */}
      <Reset />
      <GlobalStyle />
      <Greeting />
      <Switch />
    </ThemeProvider>
  );
}
```

#### 공급 받은 theme을 사용하기 위해선 타입 지정 필요 → `styled.d.ts` 파일 생성

- 아래 코드 처럼 하나씩 지정해줘도 되지만, 속성들이 추가될 가능성을 고려해서 `Theme.ts` 활용

```ts
import 'styled-components';

declare module 'styled-components' {
    export interface DefaultTheme extends Theme {
        colors: { 
            background: string; 
            text: string; 
            primary: string; 
            secondary: string; 
        }
    }
}
```

또는

```ts
import 'styled-components';
import type Theme from './Theme';

declare module 'styled-components' {
    export interface DefaultTheme extends Theme {}
}
```

#### 📌 Theme 의미 있게 사용하기 위해 참고

- [VScode Theme Color](https://code.visualstudio.com/api/references/theme-color)
- [Bootstrap Theme Color](https://getbootstrap.com/docs/5.3/customize/color/)

<br>

#### 🚨 `window.matchMedia` Error 발생

- Jest 테스트에서  다크모드 구현시 `window.matchMedia` Error 발생
- [Mocking methods which are not implemented in JSDOM](https://jestjs.io/docs/manual-mocks#mocking-methods-which-are-not-implemented-in-jsdom)

> `window.matchMedia` 는 사용자의 시스템 설정의 테마를 인식하기 위해 사용한다.

Jest 공식문서에는 `src/setupTests.ts` 파일 해당 코드 사용해서 이슈를 해결하라고 안내하고 있다.

```ts
// src/setupTests.ts

Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(), // deprecated
    removeListener: jest.fn(), // deprecated
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});
```

<br/>

## 🔗 참고

- [styled-components는 어떻게 동작할까?](https://john015.netlify.app/styled-components%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%A0%EA%B9%8C)
- [벨로퍼트와 함계하는 모던 리액트 styled-components](https://react.vlpt.us/styling/03-styled-components.html)
- [styled-components 동작 원리](https://velog.io/@code_newb/ReactJS-2.-styled-components-동작-원리)
- [62.5% Font Size Trick](https://velog.io/@shinyejin0212/62.5-Font-Size-Trick)
