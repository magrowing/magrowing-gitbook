# 2.API

## 학습 키워드

- API
- REST API
  - HTTP Method(CRUD)
- GraphQL

<br/>

## API

### 📖 API란?

- Application Programming Interface
- __클라이언트와 서버__ 사이의 데이터 전송 통신을 위한 규칙이나 룰, 매개체 역활
- 대표적으로 웹API, 라이브러리 API가 있다.

<br/>

### 📖 REST API 란?

- REST 기반으로 서비스 API를 구현한 것
- REST는 자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 __Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미__

  - URL과 HTTP 메서드를 사용하여 전체적인 구조를 나타내는 굉장히 간단한 구조 ⇒ 개발자 친화적,단순함
  - 캐싱을 지원하기에 서버에 대한 요청 수를 줄임으로써 API의 성능을 향상시킬 수 있다. ⇒ 응답에는 캐싱 가능 여부와 기간을 나타내는 캐싱 헤더가 포함되어 있기 때문
  - 구축, 테스트 및 문서화하는 데 사용할 수 있는 많은 도구와 라이브러리가 있으므로 개발 및 유지보수가 더 쉽다.
  - 웹 브라우저, 모바일 장치 및 IoT 장치 등 광범위한 클라이언트 및 서버와 호환이 가능 ⇒ 호환성 우수
  - 각 리소스는 고유 식별자(URI)를 가지고 있으며 표준 HTTP 메서드(GET, POST, PUT, DELETE) 집합을 사용하여 조작할 수 있습니다. ⇒ 리소스 지향적

<br/>

### 📖 GraphQL 란?

- API를 위한 쿼리 언어(Query Language)이며 타입 시스템을 사용하여 쿼리를 실행하는 서버사이드 런타임

  - 클라이언트가 필요한 데이터를 정확히 지정할 수 있으므로 네트워크를 통해 전송되는 데이터의 양을 줄이고 API의 효율성을 높일 수 있다.
  - 클라이언트 사이드에서 여러 리소스와 관계를 포함하는 복잡한 쿼리를 만들 수도 있으며, 쿼리 중첩을 통해 계층적 방식으로 데이터를 검색할 수 있다. ⇒ 유연한 쿼리
  - 버전관리 유용
  - RestAPI의 경우 백엔드 측에서 모든 URL을 개발하지 않으면 작업 자체가 사실상 불가능 하지만, 쿼리 테스팅을 위한 GraphiQL과 같은 여러 도구를 지원하여 개발 시간을 단축할 수 있다.

<br/>

#### REST API vs GraphQL

RestAPI의 경우 모든 엔드포인트를 일일이 개발하고 여러 경우의 수를 대응해야 하기에 개발 속도가 상대적으로 느리고, GraphQL의 경우에는 원칙적으로 Multipart 방식의 전송이 허용되지 않기에 이미지, 영상 등의 처리에 굉장히 약한 모습을 보인다.

⇒ 인증, 외부 서버와의 연동 등은 Rest API를 사용하고, 비교적 간단한 정보를 송수신 하는 경우에는 GraphQL을 사용하여 적절하게 개발의 균형을 맞추어 보자!

<br>

### ✍🏻 정리

- REST API 와 GraphQL 둘 다 클라이언트가 데이터를 서버로부터 가져오는 것을 목적한다.
- API 구조를 설계하고 데이터를 처리하기 위한 방식
- GraphQL과 REST의 장단점을 파악해 서비스에 맞는 방식을 고르는 것이 중요

<br>

### 🔗 참고

- [API란 무엇일까?](https://velog.io/@kwontae1313/API란-무엇일까)
- [REST란? REST API란? RESTful이란?](https://velog.io/@seokkitdo/Network-REST란-REST-API란-RESTful이란)
- [GraphQL이란? (REST api와 차이점)](https://hahahoho5915.tistory.com/63)
- [Rest API vs GraphQL](https://blog.toktokhan.dev/rest-api-vs-graphql-7348f54a220b)
