# 개발 환경

## 학습 키워드

* Node.js
  * `fnm(Fast Node Manager)`
* NPM(Node Package Manager)
  * package.json / package-lock.json
  * node\_modules
  * npx
* ES Modules vs CommonJS

<br/>

## 개발 환경 세팅

개발 환경을 Node.js로 세팅을 한다. toolchain이라고 부르는 개발 환경, 도구들의 모음을 Node.js로 쓴다.  
Deno를 사용하면 훨씬 간단하지만, 대부분의 서비스들은 Node.js로 구축되어 있다. Node.js를 통한 개발 환경 세팅은 생각보다 까다롭다.

어려운 이유는 도구가 계속 변화하기 때문이다. 새로운 도구가 나오면 그곳으로 쏠리고 계속 변화한다.  
강의에서는 전체적인 흐름을 파악하며 새로운 도구가 나오면 이렇게 써봐야지, 이런건 이러한 문제를 해결하려고 나왔구나 등의 접근을 해보는 것이 좋다.  

지금의 세팅도 나중에는 조금씩 달라질 것이다.  
전체적인 흐름을 파악하고 앞으로의 변경에 대응 할 수 있는 능력을 기르자.

<br/>

## JavaScript (Node.js) 개발 환경

### 📖 __[Node.js](https://nodejs.org/en)__ 무엇인가?

* Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임이다.
  * 런타임은 특정 언어로 만든 프로그램들을 실행 할 수 있는 환경을 의미한다.
* 터미널을 통해 버전 확인 할 수 있는 명령어 `node -v`

__⇒ JavaScript를 브라우저 외부에서 실행해주는 환경이다.__

* __[fnm (Fast Node Manager)](https://github.com/Schniz/fnm)__
  * 계속 업그레이드되는 Node.js로 프로젝트를 진행하다 보면 프로젝트마다 서로 다른 버전을 사용하는 경우
  * 여러 버전의 Node.js를 설치해서 사용하고 싶을 때가 있는데, [fnm](https://github.com/Schniz/fnm)을 사용하면 이게 가능하다.

<br/>

## TypeScript + ESLint + React + Jest + Parcel 개발 환경 세팅

### 1. 작업 폴더 준비

* CLI로 작업 폴더를 생성하는 방법

```shell
# 작업 폴더 생성 
mkdir my-app 

# 생성한 폴더로 이동 
cd my-app

# IDE 실행
code . 
```

<br/>

### 2. npm 패키지를 준비

#### 📖 __[npm(Node Package Manager)](https://docs.npmjs.com/, 'npm docs link')__ 무엇인가?

* Node.js의 패키지 관리 도구
* Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI(Command line interface)를 제공한다.

* __package.json / package-lock.json__
  * __package.json__
    * 현재 프로젝트에 관한 정보와 패키지 매니저(npm, yarn)을 통해 설치한 __모듈들의 의존성을 관리하는 파일__
    * __⇒ `node_modules` 폴더의 정보룰 담고 있는 파일__

    ```shell
    # package.json 파일 생성
    # 프로젝트명, 설명 등 작성할 내용이 있을 경우
    npm init 

    # package.json 파일 생성시 무조건 yes로 대답한다는 옵션
    # 입력할 내용없이 package.json 생성 
    npm init -y 
    ```

    ```json
    // JSON 포맷 형식으로 이루어짐 .json 
    {
      "name": "my-app",     // 프로젝트 이름 (소문자-이름 형태)
      "version": "1.0.0",   // 시맨틱 버전(Semantic Versioning)
      "description": "React Application Demo",
      "main": "index.js",   // npm start 실행시 해당 파일 실행 
      "scripts": {          // 실행할 수 있는 명령어, 직접 커스텀 가능
        "test": "echo \"Error: no test specified\" && exit 1" 
      },
      "author": "",
      "license": "ISC",
      "dependencies":{
        "react": "^17.0.2", // 모듈의 버전과 이름이 추가 되는 영역
      },
    }
    ```

  * __[package-lock.json](https://jihyundev.tistory.com/21, 'reference link')__  
    * 패키지의 정확한 버전을 추적하기 위한 용도의 파일
    * package.json 패키지 모듈의 정확한 버전 정보들이 담겨있는 파일
    * 프로젝트의 규모가 커질수록 package.json 파일로 버전의 정보를 관리하기에는 한계가 있어 사용된다.
    * 해당 폴더를 생성하고 관리 해야 더 많은 개발자와 협업이 가능하다.(Git 관리)

* __node_modules__

  * npm install (npm i) 명령어로 설치한 모듈과 라이브러리가 `node_modules` 폴더 안에 위치 하게 된다.

    ```shell
    # -D 없이 패키지를 설치한 경우 
    npm install react
    npm i react

    # -D 붙여서 패키지를 설치한 경우 
    npm i --save-dev typescript 
    npm i -D typescript 
    ```

  * -D 없이 패키지를 설치한 경우 `package.json "dependencies"` 영역 표기 (배포시 사용되는 패키지)
  * -D 붙여서 패키지를 설치한 경우 `package.json "devDependencies"` 영역 표기 (개발 환경에서 도구로서 사용되는 패키지)  

* __npx__
  * `node_modules` 폴더의 `.bin` 폴더 안의 패키지들을 실행 해주는 명령어
    * `./node_modules/.bin/tsc` = `npx tsc`
    * `.bin` 폴더안의 파일들은 `컴파일러`이다.
  * -D로 설치한 패키지의 경우 npx로 실행 할 수 있다.

<br/>

### 3. `.gitignore` 파일 생성

* 로컬 개발 환경에서만 사용하고 소스로 관리 되지 않아도 되는 패키지들의 정보를 기재하는 파일
* `node_modules`, `dist` , `parcel-cache`
  * [gitignore 생성 사이트](https://www.toptal.com/developers/gitignore)
  * [gitHub에서 기본 제공하는 gitignore](https://github.com/github/gitignore)

<br/>

### 4. `Typescript` 설치 및 설정

  ```shell
  # 1.타입스크립트 설치
  npm i -D typescript

  # 2.타입스크립트 컴파일러(tsc)를 실행해 초기화 ⇒ tsconfig.json 파일 생성 
  npx tsc --init
  ```

  ```json
  // 3.tsconfig.json 
  //react를 사용 할 예정이므로 주석 제거 및 jsx 이름 변경 
  "jsx": "react-jsx", 
  ```

  <br/>

### 5. `ESLint` 설치 및 설정

```shell
# 1. eslint 설치
npm i -D eslint

# 2. eslint 실행해 초기화
# eslint 조금씩 질문은 달라진다.
npx eslint --init
```

```shell
Ok to proceed? (y) 

? How would you like to use ESLint?
> To check syntax, find problems, and enforce code style

? What type of modules does your project use?
> JavaScript modules (import/export)

? Which framework does your project use?
> React

? Does your project use TypeScript?
> Yes

? Where does your code run?
> Browser

? How would you like to define a style for your project?
> Use a popular style guide

? Which style guide do you want to follow?
> XO: https://github.com/xojs/eslint-config-xo-typescript

? What format do you want your config file to be in?
> JavaScript 

? Would you like to install them now?
> Yes

? Which package manager do you want to use?
> npm
```

```json
// 4. .eslintrc.js 파일 수정 
env:{ // ...(전략)...
      jest:true // 추가 
}    

// 5. .eslintignore 파일 생성 
//.gitignore 파일 내용과 동일하게 적용하면 된다. 
```

  <br/>

### 6. `React` 설치 및 설정

``` shell
# 1.react 설치
npm i react react-dom 

# 2.typescript 사용할거라서 해당 패키지 설치  
npm i -D @types/react @types/react-dom
```

  <br/>

### 7. 테스트 도구 설치 (`Jest`)

```shell
# 1.Jest 설치
npm i -D jest @types/jest @swc/core @swc/jest \
  jest-environment-jsdom \
  @testing-library/react @testing-library/jest-dom@5.16.4
```

```javascript
// 2.jest.config.js 파일을 작성해서 테스트에 SWC를 사용하자.   
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: [
    '@testing-library/jest-dom/extend-expect',
  ],
  transform: {
    '^.+\\.(t|j)sx?$': ['@swc/jest', {
      jsc: {
        parser: {
          syntax: 'typescript',
          jsx: true,
          decorators: true,
        },
        transform: {
          react: {
            runtime: 'automatic',
          },
        },
      },
    }],
  },
  testPathIgnorePatterns: [
    '<rootDir>/node_modules/',
    '<rootDir>/dist/',
  ],
};
```

  <br/>

### 8. 번들러 도구 설치 (`Parcel`)

```shell
# 1.parcel 설치 (웹서버 띄우기 용으로)
npm i -D parcel
```

<br/>

### 9. `package.js` 파일의 scripts를 수정

* 설치된 패키지의 실행 명령어 설정

```json
"scripts": {
  "start": "parcel --port 8080",
  "build": "parcel build",
  "check": "tsc --noEmit",
  "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
  "test": "jest",
  "coverage": "jest --coverage --coverage-reporters html",
  "watch:test": "jest --watchAll"
},
```
