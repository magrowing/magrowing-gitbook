# 2. Testing Tools

## 학습 키워드

- Jest
  - Describe-Context-It 패턴
- React Testing Library

<br/>

## Test

### 📖 Test란 무엇인가?

- 프로그램을 실행하여 오류와 결함을 검출하고 애플리케이션이 요구사항에 맞게 동작하는지 검증하는 절차  

   > 내가 작성한 코드가 내가 의도한대로 동작하는지 검사하는것  by.헤다

- __Static Test (정적 테스트)__
  - 타입오류와 구문오류를 감지해 알려줘서 런타임 에러를 방지할 수 있다.
  - __테스트 도구__ : TypeScript, eslint

- __Unit Test (단위테스트)__
  - 하나의 함수, 메소드, 클래스, 모듈등이 의도한 대로 작동하는지 테스트
  - __테스트 도구__ : Jest, mocha, react-testing-library 등

- __Integration Test (통합테스트)__
  - 여러개의 모듈, 컴포넌트 등이 상호작용하여 잘 작동하는지 테스트
  - 비즈니스 로직과 연관된 테스트
  - __테스트 도구__ : react-testing-library, Enzyme 등

- __E2E Test__
  - 사용자가 어플리케이션에서 경험할 것으로 예상되는 행동을 코드로 작성해 검증하는 테스트
  - __테스트 도구__ : cypress, puppeteer 등

<br/>

## Testing Tools

- 여러 테스트 도구의 도움으로 프론트엔드 개발자가 수행해왔던 반복된 테스트를 자동화할 수 있다.
- 도구를 사용함에 따라 테스트의 유형별로 테스트가 가능해진다.  

### Jest

- [Jest 공식 문서](https://jestjs.io/)
- 페이스북에서 개발한 자바스크립트 테스팅 프레임워크
- 프론트엔드 뿐만 아니라 Node.js환경에 구축된 백엔드 애플리케션도 테스트할 수 있다.
- 단언(assertion)뿐만 아니라 모킹, 스냅샷, 테스팅, 코드 커버리지 등 다양한 API를 제공
  
### Testing Library (React Testing Library)

- [Testing Library 공식 문서](https://testing-library.com/docs/react-testing-library/intro/)
- UI를 사용자 관점에서 테스트 할 수 있도록 도와주는 라이브러리
- 실제 사용자 경험과 유사한 방식의 테스트를 하기 위한 용도로 사용  

<br/>

### 🔗 참고

- [10분테코톡 헤다의 프론트엔드 테스트 종류](https://www.youtube.com/watch?v=MN7Pw4mK6lU)
- [모던 프론트엔드 테스트 전략 — 1편](https://blog.mathpresso.com/%EB%AA%A8%EB%8D%98-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A0%84%EB%9E%B5-1%ED%8E%B8-841e87a613b2)
- [초심자를 위한 React Testing Library](https://tecoble.techcourse.co.kr/post/2021-10-22-react-testing-library/)
