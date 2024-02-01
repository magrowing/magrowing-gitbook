# 2. JSX

## 학습 키워드

- `JSX`란?
  - Syntactic sugar
  - React에서 JSX를 사용하는 목적
- React.createElement
  - React Element

<br/>

## JSX

### 📖 JSX란 무엇인가?

- JSX is an XML-like syntax extension to ECMAScript
- __JavaScript에 XML을 추가하여 확장된 문법__
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

```Javascript
// JSX을 변환한 JavaScript
React.createElement("p", null, "Hello, world!");
```

```Javascript
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

```Javascript
React.createElement(Greeting, {name:"world"});
```

```Javascript
import { jsx as _jsx } from "react/jsx-runtime";

_jsx(Greeting, {
  name: "world"
});
```

__Example #3__

```jsx
<Button type="submit">Send</Button>
```

```Javascript
React.createElement(Button, {type:"submit"}), "Send";
```

```Javascript
import { jsx as _jsx } from "react/jsx-runtime";

_jsx(Button, {
  type:"submit",
  children:"Send"
});
```

<br/>

### 🔗 참고

- [React docs JSX 소개](https://ko.legacy.reactjs.org/docs/introducing-jsx.html)
- [Syntactic Sugar](https://www.zerocho.com/category/JavaScript/post/5816c858ca15d50015d924ae)
