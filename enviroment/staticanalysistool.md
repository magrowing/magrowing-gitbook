# 4. Static Analysis Tool

## 학습 키워드

- Static Analysis Tool(정적 분석 도구)
  - ESLint
  - Prettier


<br/>

## Static Analysis Tool


### 📖 정적 분석 무엇인가?
  - 소스 코드의 실행 없이 프로그램의 문제를 찾는 과정 
  - 분석 시점이 비 런타임 환경에서 수행

<br/>

### 💡 왜 정적분석을 사용하는가?

  - 잠재적으로 버그가 발생할 수 있는 코드
  - 안티 패턴
  - 코드 스타일(컨벤션) 위반 여부
  - 성능 문제 
  - 오타 
  - 사용되지 않는 코드 
  - 잠재적인 보안 취약점 

    __⇒ 코드 스멜(code smell)이라고 불리는 문제들 발견하기 위해 사용한다__

<br/>


### 🛠️ ESLint

- 코딩 컨벤션에 위배되는 코드나 안티 패턴을 자동 검출하는 도구
- `.vscode/settings.json` 파일을 추가해 JS/TS 파일을 저장할 때마다 ESLint를 실행하고 문제점을 고치게 설정 가능 

  ```
  {
      "editor.rulers": [
          80
      ],
      "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true
      },
      "trailing-spaces.trimOnSave": true
  }
  ```

### 🛠️ Prettier

- 코드 포맷터
- 대충 문법에 맞게 코딩하고 저장 한 번 눌러주면 코드가 깔끔하기 정리 되는 기능 제공

<br/>

### ✍🏻 Static Analysis에 대한 나의 생각

- 이미 사용하고 있었다. vscode Extension을 통해 ESLint 사용하고 있었다. 하지만 이게 정적 분석 도구 인줄은 몰랐다. 자바스크립트 언어를 처음 배우고 vscode 설치시 필수로 설치하는 확장프로그램인줄만 알았는데 이것의 용도가 정적 분석 도구였구나. 
설치하고 제대로 사용하고 있는줄 알았는데 `.vscode/settings.json`파일을 옵션을 추가해 JS/TS 파일을 저장할 때마다 ESLint를 실행하고 문제점을 고치게 설정할 수 있다는 걸 알게 되었다. 그냥 쓰지말고 사용법을 제대로 익혀 제대로 활용하자! 


<br/>

### 🔗 참고

- [정적 코드 분석](https://camelsource.tistory.com/58)
- [정적 분석 Static Analysis 이란](https://hudi.blog/static-analysis/)
