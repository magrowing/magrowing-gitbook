# 2. TypeScript

## 학습 키워드

- REPL
- TypeScript
  - Interface vs Type
  - 타입 추론
  - Union Type vs Intersection Type
  - Optional Parameter

<br/>

## REPL 

### 📌 REPL은 무엇인가?
   - Read-Eval-Print-Loop의 약자 
   - 애플리케이션에서 사용자가 입력한 명령어(소스코드)를 읽고(Read) 명령어를 평가하고(Eval) 결과를 출력(Print)한 다음 다시 입력한 상태를 기다리는 상태로 돌아가는 과정을 반복(Loop)한다.

<br/>

### 💡 REPL은 왜 사용하는가? 

   - 주로 개발 진행시 소스코드를 빠르게 실행하여 결과를 즉각적으로 확인 하기 위한 용도 사용 __⇒ 테스트 용도__
   - 프로그래밍언어를 입문한 경우에 유용하게 사용
   > [ex]  C#을 배우기 위해 컴퓨터에 VisualStudio를 설치하거나 Java를 배우기 위해 Eclipse 또는 Intelij를 설치하는 과정은 입문자에게는 어려울 수 있으며, 설치 후 환경설정, 프로젝트를 관리하는 것도 굉장히 번거롭다. 이때 REPL을 사용해 언어를 익힐 수 있다. 

<br/>

### 🤖 REPL은 어떻게 사용하는가? (with ts-node)

#### 📌 ts-node는 무엇인가? 
  - TypeScript 실행기이면서 Node.js용 __REPL__
  
#### 💡 ts-node은 왜 사용하는가?
  - 타입스크립트는 __컴파일 언어__ 이기 때문에 자바스크립트로 변환이 필요하다.
  - ts-node는 타입스크립트를 자바스크립트로 변환해주기 때문에 컴파일 없이도 Node.js에서 타입스크립트를 직접 실행할 수 있음   
  __⇒ 개발 환경에서 타입스크립트 코드를 테스트 하기 위해 사용한다.__

#### 🤖 ts-node은 어떻게 사용하는가?  

   ```
   // 설치 없이 실행 
   npx ts-node

   // 종료 
   Ctrl + C or .exit
   ```

<br/>

### ✍🏻 REPL / ts-node 에 대한 나의 생각 

- typescript 기초적인 문법 공부시 typescript playground를 통해 학습했는데,vscode에서 위와 같은 방식을 사용해서 공식문서의 예제들을 테스트 해보면서 문법을 익힐 수 있다. 


<br/>

### 🔗 참고

- [REPL 참고 블로그]("https://developer-talk.tistory.com/542")
- [ts-node 참고 Gitbook]("https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week1/typescript")

<br/>

## TypeScript

### 📌 TypeScript은 무엇인가?

- __JavaScript에 타입을 부여한 언어__ __⇒JavaScript의 확장된 언어__
- Microsoft에 의해서 개발 및 관리되고 있는 오픈소스 프로그래밍 언어
- 대규모 애플리케이션을 개발하는데 JavaScript가 어렵고 불편하다는 불만에 대응하기 위해서 개발되었다.   
 __⇒ Javascript의 단점 보안 해주는 언어__

<br/>

### 💡 TypeScript은 왜 사용하는가? (장점 vs 단점 )

  -  정적 타입 언어 `에러 사전 방지`, `안전성`, `협업용이성`
  - 실행 속도 

  > JavaScript은 런타임 이후에 에러를 확인 할 수 있다. 브라우저의 개발자 도구인   `console`을 통해 에러를 확인 할 수 있다. 그러나 브라우저는 TypeScript를 실행 할 수 없다. TypeScript는 컴파일 언어이기 때문에 JavaScript로 변환하여 사용한다. 변환하는 과정 === 즉, 컴파일 하는 단계에서 에러를 확인 할 수 있다.   에러가 노출되면 Javascript 변환 되지 않아 `에러를 사전 방지 할 수 있기에 안정적이다.`

  ```
    function sum(a:number, b:number) : number{
      return a + b:
    }

    sum('10','20') // ERROE 
  ```
  > TypeScript 타입을 부여하는 언어이기에 매개변수와 반환값에 타입을 지정한다.명시적인 정적 타입 지정은 개발자의 의도를 명확하게 코드로 기술 할 수 있게 한다. 코드의 가독성을 높이기 때문에 `협업하는 과정에 유용하다.`

  
  > JavaScript는 RunTime 시 타입을 결정해서 적용 된다. 이때 오류가 있는지 확인하는 작업이 추가 되어 실행 속도가 오래 걸린다. 하지만, TypeScripts는 코드 작성 할 때 `타입을 미리 결정하기 떄문에` 오류를 확인 하는 과정을 줄여 `실행 속도가 빠르다.`
    
- 초반 환경 세팅이 불편하다. `컴파일 하기 위한 환경 세팅 어려움`
- 타입을 기재 하지 않으면 TypeError로 인한 빨간 줄 `생산성 저하`
- interface or class 등의 이름 때문에 오류 직면 `협업시 네이밍 컨벤션 필요 `
- 가독성이 상대적으로 떨어진다. `코드 길어지는 점` 
- JavaScript 에서 생기는 오류를 다 TypeScript 해결 할 순 없다. `언어 | 도구 `

<br/>


### 🤖 TypeScript은 어떻게 사용하는가? (feat.문법)

#### 1. Interface vs Type

<br/>

### 🔗 참고

- [TypeScript란 참고 블로그]("https://hymndev.tistory.com/79")
- [TypeScript장단점 참고 블로그]("https://imraccoon-developer.tistory.com/11")
