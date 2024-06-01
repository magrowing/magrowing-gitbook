# CSS 스타일링

## 학습 키워드

- 인라인 스타일(Inline Style)
- 외부 스타일(External Stylesheet)
- CSS Modules
- Tailwind CSS
- CSS-in-JS
  - Styled Components
  - Emotion
  - Vanilla Extract (zero run time)
  - Linaria (zero run time)

<br/>

## 인라인 스타일(Inline Style)

- HTML태그의 style 속성을 사용해 CSS 스타일 지정하는 방식

```html
<h2 style="font-size: 20px;">타이틀 영역</h2>
```

- 리액트에서는 style 속성으로 CSS 스타일을 지정할 때 규칙이 있다.
  - style 속성은 `객체형식의 값`으로 할당
  - font-size 와 같은 Kebab Case 형식의 속성은 `Camel Case(fontSize)` 형식으로 작성

```jsx
export default function App(){
  return <h2 style={{fontSize:'20px'}}>타이틀영역<h2>
}
```

<br/>

## 외부 스타일(External Stylesheet)

- 별도의 CSS 파일에 CSS 코드를 작성하고, 컴포넌트 파일과 연결해서 사용하는 방식
  - 전역적으로 CSS 속성이 적용되는 특징
  - `className`으로 선택자 지정

#### App.tsx

```jsx
import "./App.css";  // 👈🏻 css 파일 연결

export default function App(){
  return <h2 className="title">타이틀영역<h2>
}
```

#### App.css

```css
.title {
  font-size: 30px;
  color: #ed4848;
  text-decoration: line-through;
}
```

<br/>

## CSS Modules

- 모듈방식을 사용해서 **특정 컴포넌트에만 종속되는 CSS 코드를 작성하기 위한 방법**
  - 외부 스타일방식과 동일하게 컴포넌트 파일과 연결해서 사용
  - 파일명 `*.module.css` 으로 작성
  - 고유의 클래스네임으로 지정해주는 특징

#### App.module.css

```css
.title {
  font-size: 30px;
  color: #ed4848;
  text-decoration: line-through;
}
```

#### App.tsx

```jsx
import styles from "./App.module.css";  // 👈🏻 module.css 파일 연결

export default function App(){
  return <h2 className={styles.title}>타이틀영역<h2>
}
```

<br/>

## [Tailwind CSS](https://tailwindcss.com/docs/guides/vite)

- Utility-First 컨셉을 가진 CSS 프레임워크
  - 유틸리티 클래스를 활용한 방식
  - 프레임워크가 제공하는 유틸리티 클래스를 인라인 방식으로 적용하는 방식

#### 설치 및 초기화

```shell
npm install -D tailwindcss

# tailwind.config.js 생성
npx tailwindcss init
```

#### tailwind.config.js 옵션 변경

- src 폴더내의 파일들에 tailwind css를 사용하겠다는 의미

```json
export default {
  content: ["./src/**/*.{js,jsx,ts,tsx}"], 👈🏻
  theme: {
    extend: {},
  },
  plugins: [],
};
```

#### src/index.css

- 최상단에 위치 적용

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

#### vite.config.js

- import 및 css 옵션 추가

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react-swc';
import tailwindcss from 'tailwindcss'; // 👈🏻 추가

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  css: {
    // 👈🏻 추가
    postcss: {
      plugins: [tailwindcss()],
    },
  },
});
```

#### 사용방법

```jsx
export default function CommonButton() {
  return (
    <button className={`h-[4.4rem] text-[1.4rem] rounded-[0.8rem]`}>
      버튼
    </button>
  );
}
```

<br/>

## CSS-in-JS

- 자바스크립트 내에서 css스타일을 작성하고 관리하는 기법
  - 자바스크립트 내에서 정의하고 컴포넌트와 함께 번들링하는 접근방식
  - 컴포넌트 기반의 스타일링, 스타일의 범위를 컴포넌트로 한정하며, 스타일 충동을 방지하는 등의 이점
  - CSS-in-JS 방식의 라이브러리들 존재

### [Styled Components](https://styled-components.com/)

#### 설치

```shell
npm i styled-components
```

#### 사용방법

```tsx
import styled from 'styled-components';

const HelloWorld = styled.h1`
  font-size: 30px;
  color: #ed4848;
  text-decoration: line-through;
  &:hover {
    color: blue;
  }
`;

export default function App() {
  return <HelloWorld>Hello, World!</HelloWorld>;
}
```

<br/>

### [Emotion](https://emotion.sh/docs/introduction)

- 아직 인터넷익스플로러 11이 사용량을 보이던 시절 인기가 높아졌던 CSS-in-JS라이브러리
  - styled-components는 IE11을 지원하려면 별도의 폴리필을 사용해야했는데 emotion은 별도의 폴리필 없이도 IE11을 지원했다.

#### 설치

```shell
npm i @emotion/css
```

#### 사용방법

> 📌 사용해보지 않아서, 사용 후 별도로 정리 예정

<br/>

### vanilla extract

- 제로 런 타임(Zero Run Time)이라는 개념이 새롭게 등장하게 되면서 최근에 급 부상하고 있는 라이브러리
  - Zero Run Time : 런 타임 오버헤드가 없는 CSS를 의미

> 📖 런타임 오버헤드란? <br/> 프로그램이 실행되는 동안 추가적인 연산이나 자원소비를 의미<br/> 런타임 오버헤드가 발생하면 메모리 사용량이 증가하고, CPU 사용량 증가하며, 응답시간지연이 된다는 성능적 단점 존재

#### 설치

```shell
npm i @vanilla-extract/css @vanilla-extract/vite-plugin
```

#### vite.config.js

- 빌드 과정에서 CSS 파일을 생성하기 위해 vite와 통합할 수 있는 플러그인을 사용

```javascript
import { defineConfig } from 'vite';
import { vanillaExtractPlugin } from '@vanilla-extract/vite-plugin'; // 👈🏻 추가

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react(), vanillaExtractPlugin()], // 👈🏻 추가
});
```

#### 사용방법

> 📌 사용해보지 않아서, 사용 후 별도로 정리 예정

<br/>

### Linaria

- 4년전 유행했던 제로 런 타임 기반 CSS 라이브러리

#### 설치

```shell
npm install @linaria/core @linaria/react @wyw-in-js/babel-preset @wyw-in-js/vite
```

#### vite.config.js

```javascript
import { defineConfig } from 'vite';
import wyw from '@wyw-in-js/vite'; // 👈🏻 추가

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react(), wyw()], // 👈🏻 추가
});
```

#### 사용방법

> 📌 사용해보지 않아서, 사용 후 별도로 정리 예정

<br/>

## 👩🏻‍💻 컴포넌트 CSS 스타일 적용 실습

주어진 피그마 디자인을 보고, 디자인과 같은 버튼을 재사용할 수 있는 컴포넌트로 생성

#### 조건

- 컴포넌트 1개로 디자인처럼 여러개의 버튼을 생성
- 버튼의넓이(높이는고정), 배경, 색상, 글자색은 자유롭게 변경 가능해야한다. (props 활용)

### 1. CSS Modules

- 🔗 [실습링크](https://github.com/magrowing/Project-Camp-Next-Homework/commit/b0fb5b9398904e98cfa31339990448210e43ef15)

#### ModuleButton.module.css

```css
.moduleBtn {
  width: 100%;
  height: 4.4rem;
  line-height: 4.4rem;
  font-size: 1.4rem;
  border-radius: 0.8rem;
}
```

#### ModuleButton.tsx

```jsx
import { ReactNode } from 'react';

import styles from './ModuleButton.module.css';

type ModuleButtonProps = React.ComponentProps<'button'> & {
  children: ReactNode,
  style: {
    [key: string]: string,
  },
};

export default function ModuleButton(props: ModuleButtonProps) {
  const { children, style, ...restButtonProps } = props;
  return (
    <button style={style} className={styles.moduleBtn} {...restButtonProps}>
      {children}
    </button>
  );
}
```

### 2. styled-components

- 🔗 [실습링크](https://github.com/magrowing/Project-Camp-Next-Homework/commit/5f846f7d6c297d2922b76ed798b3bcb8c216ce2c)

#### StyledButton.tsx

```jsx
import { ReactNode } from 'react';

import styled from 'styled-components';

const Button = styled.button`
  width: 100%;
  height: 4.4rem;
  line-height: 4.4rem;
  font-size: 1.4rem;
  border-radius: 0.8rem;
`;

type StyledButtonProps = React.ComponentProps<'button'> & {
  children: ReactNode,
  style: {
    [key: string]: string,
  },
};

export default function StyledButton(props: StyledButtonProps) {
  const { style, children, ...restButtonProps } = props;
  return (
    <Button style={style} {...restButtonProps}>
      {children}
    </Button>
  );
}
```

### 3. Tailwind CSS

- 🔗 [실습링크](https://github.com/magrowing/Project-Camp-Next-Homework/commit/e7119a45dc3ad9ac583d0b819e391763cadf552e)

#### TailwindButton.tsx

```jsx
import { ReactNode } from 'react';

type TailwindButtonProps = React.ComponentProps<'button'> & {
  children: ReactNode,
  style: {
    [key: string]: string,
  },
};

export default function TailwindButton(props: TailwindButtonProps) {
  const { children, style, ...restButtonProps } = props;
  return (
    <button
      style={style}
      {...restButtonProps}
      className="w-[100%] h-[4.4rem] leading-[4.4rem] text-[1.4rem] rounded-[.8rem]"
    >
      {children}
    </button>
  );
}
```

#### 🧐 TailwindCSS를 적용하면서 알게된 부분

props로 전달받은 style을 동적으로 적용할 수 있을거라고 생각했는데... 이렇게 적용하면 안된다. 클래스의 이름을 동적으로 구성하지말라고 공식문서에 기재되어 있었다.

- [공식문서](https://tailwindcss.com/docs/content-configuration#dynamic-class-names)

```jsx
import { ReactNode } from 'react';

type TailwindButtonProps = React.ComponentProps<'button'> & {
  children: ReactNode,
  style: {
    [key: string]: string,
  },
};

export default function TailwindButton(props: TailwindButtonProps) {
  const { children, style, ...restButtonProps } = props;
  return (
    <button
      {...restButtonProps}
      className={`w-[${style.width}] bg-[${style.backgroundColor}] text-[${style.color}] h-[4.4rem] leading-[4.4rem] text-[1.4rem] rounded-[.8rem]`}
    >
      {children}
    </button>
  );
}
```
