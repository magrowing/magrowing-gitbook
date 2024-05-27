# 변수

## 학습 키워드

- 자바스크립트의 역사
- 자바스크립트 선언 위치
- 변수

<br/>

## 자바스크립트의 역사

### 자바스크립트의 탄생

1995년 당시 90%의 시장 점유율로 웹 브라우저 시장을 지배하고 있던 Netscape는 정적인 HTML문서를 동적으로 표현하기 위해 경량의 프로그래밍 언어를 도입하기로 결정했고, 그렇게 탄생한 것이 자바스크립트이다.
초기 이름 `Mocha`이였고, 같은 해 `LiveScript`로 이름을 변경하였고, 당시 대유행하던 Java의 인기에 편승하기위해 LiveScript의 이름을 JavaScript로 변경했다.

### 자바스크립트의 파편화

이렇게 탄생하게 된 자바스크립트는 오늘날의 표준 프로그래밍 언어가 되기까지는 순탄하지만은 않았다.<br/>
같은 해에 Microsoft에서는 Netscape의 성공을 보고 브라우저 흥행을 예측했다.
자신들만의 브라우저를 갖고자 한 MS는 자바스크립트의 파생 버전 `JScript`를 `인터넷 익스플로러`에 탑재하여 출시하였다.

그런데 문제는 자사 브라우저의 시장 점유율을 점유하기 위해 자사 브라우저서만 동작하는 기능을 경쟁적으로 추가하기 시작했다는 것이다.
이로 인해 모든 브라우저에서 동작하는 웹 페이지를 개발하는 것은 무척 어려워졌다. 브라우저에 따라 정상적으로 동작하지 않는 크로스브라우징 이슈가 발생하기 시작했고, 이떄 부터 서로 다른 브라우저에서 동일하게 동작하는 웹사이트를 만들어야 하는 개발자들은 고통을 받아 표준화된 자바스크립트에 대한 필요성이 제기되기 시작하였다.

보다 못한, Netscape는 컴퓨터 시스템의 표준을 관리하는 비영리 표준화 기구인 `ECMA International`에 자바스크립트의 표준화를 제안했고, 1997년 ECMAScript 1 출시된다.

> 📖 ECMAScript란 <br/>브라우저에서 동작하는 언어를 만들 때 엔진이 이해할 수 있도록 변수를 만드는 방법이나 함수 정의하는 방법 등을 정리한 문서

2000년대에 들어서자 그사이에 MS의 IE 브라우저가 점유율 95%를 차지하며 MS사의 콧대는 하늘을 찔렀다.
결국 그들은 자신들의 IE 브라우저가 곧 표준임을 선언하며 ECMA에 참여하지 않았다.
그래서 2000년대 ECMAScript 4 이후 표준안 진행이 매우 더뎌졌다.

### 브라우저들의 경쟁, JQuery 등장

2004년 mozilla사에서 `FireFox` 브라우저를 출시했다. `ActionScript`라는 언어를 사용했고, 이것으로 표준안을 다시 만들어 보자고 제안했으나, `JScript`와 `JavaScript`와는 너무나도 달라서 신흥강자를 기준으로 새로운 표준안을 진행하기에는 무리가 있었다.
결국 표준안을 두고 3사의 치열한 경쟁이 이어졌다. 그 와중에도 `Opera`,`Safari`와 같은 다른 브라우저들이 탄생하며 경쟁은 더욱 심화되었다.

서로 다른 브라우저들의 호환성 문제를 해결하기 위해 개발자들 사이에서는 커뮤니티가 형성되었고, 그 사이에 jQuery, dojo, mootools같은 라이브러리가 등장했다. 그 중에서도 특히 JQuery가 흥행했다.

### Chrome의 등장

웹시장을 급격하게 바꾸는 진취적인 사건이 발생하게 된다.
그것은 바로 2008년 Google에서 Chrome 브라우저를 출시했다.
이 브라우저는 V8 자바스크립트 엔진을 포함하고 있었는데 빠른 성능을 보여주게 되면서 점유율 장악하게 된다.

결국 위기감을 느낀 기업들은 2008년 7월에 이르러서야 윈윈관계를 형성하며 생산적인 대화를 시작한다.

### 자바스크립트의 표준화

2009년 ECMAScript 5, 그리고 2015년 ECMAScript 6가 완성되며 급격한 변화를 이뤄낸 자바스크립트는 표준화된 언어가 되었다.
더이상 jQuery같은 라이브러리의 도움 없이도 충분히 JS와 웹APIs에서 제공하는 API들 만으로 모든 브라우저에서 동작하는 웹사이트를 만들 수 있게 되었고, 브라우저서만 동작하던 자바스크립트를 브라우저 이외의 환경에서 동작시킬 수 있는 자바스크립트 `Node.js`의 등장으로 자바스크립트는 이제는 프론트엔드 영역은 물론 백엔드 영역까지 아우르는 웹 프로그래밍의 표준으로 자리잡고 있다.

<br/>

## 자바스크립트 선언 위치

- 브라우저는 HTML 문서를 파싱해 읽다가 자바스크립트 만나면 진행하고 있던 파싱을 멈추고 JS파일을 서버에서 다운 받고 파싱하고 실행한 뒤에 다시 HTML 문서를 파싱하는 순으로 동작한다.

### `<head>` 태그 안에 작성 할 경우

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Document</title>
  <script scr="./index.js"></script>
</head>
```

- 스크립트를 다운받고 실행하는 동안 파싱이 중단되기 때문에 스크립트가 크다면, 사용자가 웹사이트를 보기까지 오랜 시간이 소요될 수 있다.
- 스크립트에 DOM 요소를 사용하는 코드가 있다면 아직 html이 파싱되기 전이므로 기능이 정상적으로 동작하지 않을 수 있다.

### `<body>` 태그 안에 작성 할 경우

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <script scr="./index.js"></script>
  </body>
</html>
```

- HTML 문서를 순차적으로 파싱해서 읽어 페이지 로드는 빠르지만, 자바스크립트의 의존성이 큰 페이지인 경우, 정상적으로 동작하기까지 오랜 시간이 소요될 수 있다.

### async 속성

- HTML 파싱과 병렬적으로 스크립트를 다운받고, 다운받은 순서에 따라 스크립트를 실행한다. 이때, 스크립트를 실행하는 순서는 보장되지 않으며, HTML 파싱이 진행 중이라면 파싱을 중단하고 스크립트를 실행한다.

```html
<script async scr="./index.js"></script>
```

> ✅ HTML 파싱하는 도중에 파싱을 중단하고 스크립트가 실행 될 수 있기 때문에 DOM요소를 조작하는 코드가 있다면 기능이 제대로 동작하지 않을 수 있다. 또한 순서가 보장되어야 하는 스크립트들을 포함할 경우 적절하지 않다.

### defer 속성

- html 파싱과 병렬적으로 스크립트를 다운받고, HTML 파싱을 모두 진행한 후에 문서에 포함된 순서에 따라 스크립트를 실행한다.

```html
<script defer scr="./index.js"></script>
```

> ✅ async와 다르게 HTML 파싱을 모두 마친 후에 스크립트를 실행하기 때문에 DOM 요소를 사용하는 코드가 제대로 동작하지 않을 가능성의 문제가 해결된다. 또한 스크립트를 다운받은 순서가 아닌 문서에 포함된 순서로 실행하기 때문에 스크립트들의 실행순서가 보장된다.

<br/>

## 변수

- 모든 프로그래밍 언어의 궁극적인 목표는 데이터 처리이다. 자바스크립트에서 변수를 선언한다는 것은 데이터를 저장할 공간을 만드는것을 의미한다.

### 변수의 선언 방법

#### var

- ES6이전에 주로 사용되던 변수 선언 키워드
- 중복 선언 가능
  - 기존에 선언해둔 변수의 존재를 잊고 재선언하게 된다면 예측하지 못한 문제 발생 → 이를 보안하기 위해 ES6부터 `let`, `const` 탄생
- 재할당 가능

```javascript
var name = 'javascript';
console.log(name); // javascript

var name = 'react';
console.log(name); // react
```

#### let

- 중복 선언 불가능
- 재할당 가능

```javascript
let name = 'javascript';
console.log(name); // javascript

let name = 'react';
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared
// 🚨 해당 변수가 이미 선언되었다는 에러 메시지가 출력됨

name = 'vue';
console.log(name); // vue
```

#### const

- 중복 선언 불가능
- 재할당 불가능

```javascript
const name = 'javascript';
console.log(name); // javascript

const name = 'react';
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared
// 🚨 해당 변수가 이미 선언되었다는 에러 메시지가 출력됨

name = 'vue';
console.log(name);
// Uncaught TypeError: Assignment to constant variable
// 🚨 해당 변수에 재힐당 할수 없다는 의미
```

> ✅ 값만 재할당이 불가할 뿐 Reference DataType의 속성은 변경할 수는 있다.

```javascript
const obj = {
  name: '홍길동',
};
obj.age = 20;
console.log(obj); // { name: '홍길동', age: 20 }
```

<br/>

### 🔗 참고

- [자바스크립트의 탄생 이유와 역사](https://velog.io/@dongjun187/자바스크립트JavaScript의-탄생-이유와-역사)
- [자바스크립트의 역사](https://velog.io/@jmkacc99/JavaScript-자바스크립트의-역사)
- [태그의 async, defer 속성, 차이](https://han-study.tistory.com/11#4.%20defer%20%EC%86%8D%EC%84%B1)
- [async, defer 의미, 실행순서, 공통점, 차이점](https://webroadcast.tistory.com/15)
- [var, let, const 차이점](https://80000coding.oopy.io/e1721710-536f-43f2-823b-663389f5fbfa)
