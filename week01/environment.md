# 1. 개발 환경

## 학습 키워드

* Node.js
  * `fnm(Fast Node Manager)`
* NPM(Node Package Manager)
  * package.json / package-lock.json
  * node\_modules
  * npx
* ES Modules vs CommonJS





## 개발 환경 세팅

개발 환경을 Node.js로 세팅을 한다. toolchain이라고 부르는 개발 환경, 도구들의 모음을 Node.js로 쓴다.  
Deno를 사용하면 훨씬 간단하지만, 대부분의 서비스들은 Node.js로 구축되어 있다. Node.js를 통한 개발 환경 세팅은 생각보다 까다롭다.


어려운 이유는 도구가 계속 변화하기 때문이다. 새로운 도구가 나오면 그곳으로 쏠리고 계속 변화한다.  
강의에서는 전체적인 흐름을 파악하며 새로운 도구가 나오면 이렇게 써봐야지, 이런건 이러한 문제를 해결하려고 나왔구나 등의 접근을 해보는 것이 좋다.  
지금의 세팅도 나중에는 조금씩 달라질 것이다.  
바뀐 부분이 있으면 문서를 찾아서 업데이트 하면 되고 새로운 도구를 선택할 수도 있다.  
  
  
  
    
  
  
## JavaScript (Node.js) 개발 환경

Node.js를 설치하고, 프로젝트를 진행할 수 있는 Node.js 패키지를 만들고,  
코드 퀄리티를 일정 수준 이상으로 유지할 수 있도록 lint와 test를 실행할 수 있는 상태를 만든다.

  __⇒ JavaScript를 브라우저 외부에서 실행할 수 있게 해주는 런타임 환경이다.__

* __[Node.js](https://nodejs.org/en)__
  * Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임이다. 
    * 런타임은 특정 언어로 만든 프로그램들을 실행 할 수 있는 환경을 의미한다.
  * 터미널을 통해 버전 확인 할 수 있는 명령어 `node -v`
* __[fnm (Fast Node Manager)](https://github.com/Schniz/fnm)__
  * 계속 업그레이드되는 Node.js로 프로젝트를 진행하다 보면 프로젝트마다 서로 다른 버전을 사용하는 경우
  * 여러 버전의 Node.js를 설치해서 사용하고 싶을 때가 있는데, [fnm](https://github.com/Schniz/fnm)을 사용하면 이게 가능하다.


> [실습] 공식문서보다 해당문서를 참고해 node.js 버전을 최신 버전으로 변경해주었다.  
[fnm (Fast Node Manager) 설치 방법](https://github.com/ahastudio/til/blob/main/javascript/20181212-setup-javascript-project.md)


  
  
  
  
  

## TypeScript + React + Jest + ESLint + Parcel 개발 환경 세팅

### 1.작업 폴더 준비
  * CLI로 작업 폴더를 생성하는 방법
```
// 작업 폴더 생성 
mkdir my-app 

// 생성한 폴더로 이동 
cd my-app

//IDE 실행
code . 
```

### 2.npm 패키지를 준비 

* __[npm(Node Package Manager)](https://docs.npmjs.com/, 'npm docs link')__
  * Node.js의 패키지 관리 도구
  * Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI(Command line interface)를 제공한다. 

* __package.json / package-lock.json__
  * __package.json__
    * 현재 프로젝트에 관한 정보와 패키지 매니저(npm, yarn)을 통해 설치한 모듈들의 의존성을 관리하는 파일
    * __⇒ `node_modules` 폴더의 정보룰 담고 있는 파일__
  ```
    // JSON 포맷 형식으로 이루어짐 .json 
    {
      "name": "my-app",     // 프로젝트 이름 (소문자-이름 형태)
      "version": "1.0.0",   // 시맨틱 버전(Semantic Versioning)
      "description":  {
        "react": "^17.0.2", // 모듈의 버전과 이름이 추가 되는 영역 
      },
      "main": "index.js",   // npm start 실행시 해당 파일 실행 
      "scripts": {          // 실행할 수 있는 명령어, 직접 커스텀 가능
        "test": "echo \"Error: no test specified\" && exit 1" 
      },
      "author": "",
      "license": "ISC"
  }
  ```
  * CLI를 통해 실행 하는 방법 
  ```
   // package.json 파일 생성
   // 프로젝트명, 설명 등 작성할 내용이 있을 경우
   npm init 

   // package.json 파일 생성시 무조건 yes로 대답한다는 옵션
   // 입력할 내용없이 package.json 생성 
   npm init -y 
  ```
  * __[package-lock.json](https://jihyundev.tistory.com/21, 'reference link')__  
    * 패키지의 정확한 버전을 추적하기 위한 용도의 파일 
    * 프로젝트의 규모가 커질수록 package.json 파일로 버전의 정보를 관리하기에는 한계가 있어 사용된다. 
    * 해당 폴더를 생성하고 관리 해야 더 많은 개발자와 협업이 가능하다.  
    * __⇒ 규모가 큰 프로젝트의 경우, 여러 명의 개발자들이 동일한 개발 환경을 가지는 것이 중요하다.  
     그렇기에 해당 파일의 관리를 통해 패키지들의 버전의 히스토리를 가지고 있는 파일이라고 우선 정의하자.__

* __node_modules__
  * npm install (npm i) 명령어로 설치한 모듈과 라이브러리가 `node_modules` 폴더 안에 위치 하게 된다.
 




