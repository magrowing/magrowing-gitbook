# 3.Express

## 학습 키워드

- Express

<br/>

## Express

### 📖 [Express](https://expressjs.com/ko/)란?

> 🤔 어떤 개발자가 node.js를 사용해서 서버를 만들고자 한다.서버를 만들기 위해선 HTTP 통신을 위한 여러가지 설정을 해줘야 하는 과정은 복잡하다.그래서 쉽게 서버를 구성 할 수 있게 만든 프레임워크 Express를 사용하는 것이다.

- Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크
- __Node.js를 사용하여 쉽게 서버를 구성할 수 있게 만든 클래스와 라이브러리의 집합체__
- 웹 어플리케이션, API 개발을 위해 설계 되었다.

<br/>

### 🤖 간단한 서버 앱 npm 패키지 세팅 (feat.Typescript)

1. 작업 폴더 준비

```shell
mkdir express-demo-app
cd express-demo-app
```

2. 패키지 초기화

```shell
npm init -y
```

3. .gitignore 파일 생성

echo CLI 명령어를 통해 node_modules 제외 처리

```shell
touch .gitignore
echo "/node_modules/" > .gitignore
```

4. TypeScript 설치

```shell
npm i -D typescript
npx tsc --init
```

5. ts-node 설치

컴파일을 없이 Node.js에서 개발 환경에서 Typescript 실행 하기 위해 설치

```shell
npm i -D ts-node
```

6. ESLint

```shell
npm i  -D eslint
npx eslint --init
```

```shell
? How would you like to use ESLint? …
❯ To check syntax, find problems, and enforce code style

? What type of modules does your project use? …
❯ JavaScript modules (import/export)

? Which framework does your project use? …
❯ None of these 

? Does your project use TypeScript?
❯ Yes

? Where does your code run? …
✔ Node 

? How would you like to define a style for your project? …
❯ Use a popular style guide

? Which style guide do you want to follow? …
❯ XO: https://github.com/xojs/eslint-config-xo-typescript

? What format do you want your config file to be in? …
❯ JavaScript

? Would you like to install them now with npm?
❯ Yes

? Which package manager do you want to use? … 
❯ npm
```

7. Express 설치

```shell
npm i express
npm i -D @types/express
```

<br>

### Hello World 예제

1. app.ts 파일 생성 및 코드 작성

```shell
touch app.ts 
```

```ts
import express from 'express';

const port = 3000;

const app = express();

app.get('/', (req, res) => {
    res.send('Hello, world!');
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
});
```

2. ts-node로 실행

```shell
npx ts-node app.ts
```

3. 코드를 수정할 때마다 서버를 재실행해야 하는 문제를 피하기 위해 nodemon 설치

- [nodemon](https://github.com/remy/nodemon)
- ts-node에 대한 의존성이 있기 때문에 꼭 ts-node 설치 후 사용
- 실제 서비스에서는 사용하지 않는다.

```shell
npm i -D nodemon
npx nodemon app.ts
```

4. package.json 파일 scripts 추가

```json
 "scripts": {
    "start": "nodemon app.ts",
    "lint": "eslint --fix ."
  },
```

<br>

### 🔗 참고

- [express란?](https://velog.io/@madpotato1713/JAVASCRIPT-express란)
