# MSW

## 학습 키워드

- Service Worker
- MSW(Mock Service Worker)
- polyfill(폴리필)

<br/>

## Service Worker

### 📖 [Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)

- 웹 응용 프로그램, 브라우저, 그리고 (사용 가능한 경우) 네트워크 사이의 __프록시 서버 역활 수행__
- 서비스 워커는 출처와 경로에 대해 등록하는 이벤트 기반 워커로서 JavaScript로 작성 된 파일

서비스 워커는 연관된 웹 페이지/사이트를 통제하여 탐색과 리소스 요청을 가로채 수정하고, 리소스를 굉장히 세부적으로 캐싱힌다.

__⇒ 웹 앱이 어떤 상황에서 어떻게 동작해야 하는지 완벽하게 바꿀 수 있다. (대표적인 상황은 네트워크를 사용하지 못하는 경우)__

<br/>

## MSW(Mock Service Worker)

### 🌎 MSW 사용 배경(feat.Mocking)

프론트엔드 개발을 진행하다보면 백엔드 개발과의 종속적인 부분이 생기기 마련이다. (대표적으로 백엔드의 API를 활용해야 하는 경우)

> 기획자 : XX 작업은 어떻게 진행 중인가요? <br/>
프론트엔드 개발자 : 그게… 아직 API가 준비되지 않아서 다음 주까지는 기다려야 합니다.....

예상한 기간보다 API 개발에 시간이 더 필요해진 경우, 그 시간만큼
프론트엔드 개발자가 개발을 진행하지 못하는 상황이 생겨나기도 한다.

__그래서 Mocking을 통해 위의 문제를 해결하고자 했다.__

[Think in React 예제를 통해 API 요청 코드 모킹](https://magrowing.gitbook.io/magrowing-gitbook/category/testing/react_testing_library#mocking)처럼 직접적으로 내부 로직에 직접 Mocking해서 필요한 화면에 붙이는 방식이 있지만,

- 서비스 로직에 직접 Mocking을 해야 하므로 애플리케이션 서비스 로직을 수정 필요
- HTTP 메소드와 네트워크의 응답 상태에 따라 각각 대응하기가 쉽지 않다.
- (Mocking으로 만든 결과물)화면에 대한 테스팅 및 디버깅 시에 어려움이 발생 .....

💡 결국 실제 API를 사용하는 것처럼 네트워크 수준에서 Mocking 하길 원하기 떄문에 __네트워크 요청 과정에서 Request에 대한 Mocking이 가능한 MSW를 사용하는 것이다.__

### 📖 [MSW](https://v1.mswjs.io/)는 무엇인가?

- Mock Service Worker의 약자
- API Mocking 라이브러리
- 네트워크 요청을 가로채서 모의 응답(Mocked response)을 보내주는 역할을 수행

### ⚙️ MSW 설치 및 설정

- [Set up Mock Service Worker in Node.js](https://mswjs.io/docs/integrations/node)

#### MSW 패키지 설치

```shell
npm i -D msw@0.36.4 
```

<br/>

## 🔗 참고

- [Web Worker](https://velog.io/@whow1101/Web-Worker)
- [서비스 워커에 대해 알아보고 Mock Response 만들기](https://fe-developers.kakaoent.com/2022/221208-service-worker/)
- [⭐️ Mocking으로 생산성까지 챙기는 FE 개발](https://tech.kakao.com/2021/09/29/mocking-fe/)
