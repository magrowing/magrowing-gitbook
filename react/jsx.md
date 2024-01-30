# 2. JSX

## 학습 키워드

- `JSX`란?
  - Syntactic sugar
  - React에서 JSX를 사용하는 목적
- React.createElement
  - React Element
- React StrictMode

<br/>

### 📖 JSX란 무엇인가?

- JSX is an XML-like syntax extension to ECMAScript
- __JavaScript에 XML을 추가한 확장한 문법__
- JavaScript XML의 줄임말
- 공식적인 자바스크립트 문법은 아니다.
- JSX 하나의 파일에 자바스크립트와 HTML을 동시에 작성 할 수 있다.
- React에서만 사용되는게 아니며, Vue.js등 다른 프레임워크에서도 사용 가능하다.

#### 📖 Syntactic sugar는 무엇인가?

- 문법적인 설탕
- 프로그래밍 언어 차원에서 제공되는 논리적으로 간결하게 표현하는 것
- 직관적이지는 않지만 코드의 양을 줄여주는 것들을  Syntactic sugar 라고 부른다.

> ES2015에서는 여러가지 Syntactic sugar가 추가 되었다고 평가 된다.<br/>
예로 Class, 객체분해, spread(...)

#### 📌 React에서 JSX를 사용하는 목적
>
> __JSX는 React.createElement(component, props, ...children)함수를 호출하기 위한 Syntactic sugar로서 역활__ 을 한다.

- React에서 JSX 사용이 필수는 아니며 빌드 환경에서 컴파일 설정을 하고 싶지 않을 때 JSX 없이 React를 사용하면 편리하다.  
- JSX로 할 수 있는 모든 것은 순수 JavaScript로도 할 수 있다.

<br/>

### 🤖 JSX 어떻게 사용하는가? (feat.기본 규칙)

- [JSX 이해하기](https://ko.legacy.reactjs.org/docs/jsx-in-depth.html)
- JavaScript 표현식만을 `{}` 중괄호 안에 넣어서 사용 할 수 있다.
- 태그가 꼭 닫혀야 한다. <Img />
- 1개의 최상위의 태그가 존재해야한다.
  - <></> , `React.Fragement`

<br/>

### 🔄  React.createElement (JSX 없이 사용하는 React)

- JSX는 브라우저에서 실행하기 위해선 일반적인 자바스크립트 코드로 변환해야 한다.
- JSX → JavaScript 코드로 Babel을 통해 변환 했을 시 __React.createElement__ 를 통해 `React Element`를 생성 할 수 있다.

__Example #1__

```jsx
// JSX
<p>Hello, world!</p>
```

```JavaScript
// JSX을 변환한 JavaScript
React.createElement("p", null, "Hello, world!");
```

```JavaScript
//the New JSX Transform
import { jsx as _jsx } from "react/jsx-runtime";

_jsx("p", {
  children: "Hello, world!"
});
```

__Example #2__

```jsx
<Greeting name="world" />
```

```JavaScript
React.createElement(Greeting, {name:"world"});
```

```JavaScript
import { jsx as _jsx } from "react/jsx-runtime";

_jsx(Greeting, {
  name: "world"
});
```

__Example #3__

```jsx
<Button type="submit">Send</Button>
```

```JavaScript
React.createElement(Button, {type:"submit"}), "Send";
```

```JavaScript
import { jsx as _jsx } from "react/jsx-runtime";

_jsx(Button, {
  type:"submit",
  children:"Send"
});
```

<br/>

### 📖 React StrictMode란 무엇인가?

- [React docs Strict Mode](https://ko.legacy.reactjs.org/docs/strict-mode.html)

> 자바스크립트에서는 엄격 모드가 있다. 코드 파일 상단에 "use strict"를 써 놓으면 자바스크립트를 실행할 때 조금 더 엄격하게 코드를 검사한다. 리액트에도 이와 유사한 목적으로 사용하는  `<StrictMode />`라는 컴포넌트가 있다.

- 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구
- Fragment와 같이 UI를 렌더링하지 않으며, 자손들에 대한 부가적인 검사와 경고를 활성화한다.
- Strict 모드는 개발 모드에서만 활성화되기 때문에, 프로덕션 빌드에는 영향을 끼치지 않는다.
- StrictMode는 아래와 같은 부분에서 도움이 된다.
  - 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
  - 레거시 문자열 ref 사용에 대한 경고
  - 권장되지 않는 findDOMNode 사용에 대한 경고
  - 예상치 못한 부작용 검사
  - 레거시 context API 검사
  - Ensuring reusable state

> Strict 모드를 사용하면 리액트가 자식 컴포넌트를 검사하고 잘못 사용된 부분을 우리에게 알려준다. <br/> 이런 경고 메세지들은 어플리케이션에 잠재된 문제를 해결 할 수 있도록 도와준다.

### ✍🏻 React StrictMode에 대한 나의 생각

- 개발단계에서 사용하면 좋지 않을까싶다. 배포 했을 경우 해당 모드가 영향을 끼지지는 않는다고 했으니, 개발하는 과정에서 문제를 미리 발견하면 좋은거니깐 앞으로 개발단계에서 사용해보자!

<br/>

### 🔗 참고

- [React docs JSX 소개](https://ko.legacy.reactjs.org/docs/introducing-jsx.html)
- [Syntactic Sugar](https://www.zerocho.com/category/JavaScript/post/5816c858ca15d50015d924ae)
- [리액트 StrictMode](https://jeonghwan-kim.github.io/2022/05/20/react-strict-mode)
