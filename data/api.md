# 2.API

## 학습 키워드

- URL구조
- API
- REST API
  - HTTP Method(CRUD)
- GraphQL
- Fetch API
  - Promise
  - ReableStream
  - Unicode
- CORS

<br/>

## API

### 🌎 URL 구조

- Uniform Resource Locators 의 약자
- HTML,css 문서, 이미지 등 __리소소의 위치를 나타내는 주소__
- URL은 Scheme(=Protocol), Domain Name(=IP), Port, Path, Parameter, Anchor로 이루어져 있다.

<br/>

![URL](./image/url.png)

1. __Scheme(=Protocol)__

- 브라우저가 리소스를 요청하기 위해 사용해야 하는 프로토콜
- 웹에서는 브라우저와 서버 간에 데이터를 주고 받기 위한 방식으로 `HTTP/HTTPS` 프로토콜이 가장 많이 사용 됨
- HTTPS/HTTP 외에도 mailto:(이메일 주소를 지정하는 프로토콜), ftp:(파일을 주고 받는 프로토콜) 등 다양한 프로토콜이 존재
  - HTTP(Hyper Text Transfer Protocol): 웹 브라우저와 웹 서버가 서로 데이터를 주고받기 위해 만든 통신규약
  - HTTPS(Hyper Text Transter Protocol Secure): HTTP에서 보안이 강화된 버전

> 프로토콜이란? <br/>
중앙 컴퓨터와 단말기 사이에서 데이터 통신을 원활하게 하기 위해 필요한 통신 규약

2. __Domain Name (도메인)__

- 컴퓨터와의 통신에서는 숫자로 표현된 주소(=IP)를 사용하기 때문에 사용자가 쉡게 기억하고 찾을 수 있도록 만들어준 서비스가 도메인이다.

> <https://www.naver.com> <br/>
(호스트명(차상위 도메인/서브 도메인) - 도메인명 - 최상위 도메인명)

- www: 호스트명(차상위 도메인/서브 도메인)
  - 보조 도메인으로써 URL로 전송하거나 계정 내의 IP 주소나 디렉토리로 포워딩되는 도메인 이름의 확장자

- naver: 도메인명
  - 임의로 지정할 수 있는 사이트의 이름
  - 사용자에게 쉽게 기억 될 수 있도록 서비스명으로 지정에서 사용
- com: 최상위 도메인명
  - 도메인 레벨 중 가장 높은 단계에 있는 도메인
  - 도메인의 목적, 종류, 국가를 나타낸다.

> localhost : 자신의 컴퓨터를 의미(컴퓨터 네트워크에서 사용하는 루프백)

3. __Port__

- 포트 번호를 통해 어떤 서버를 이용할지 결정하며, : (콜론) 뒤에 나온다
- URL에는 기본적으로 표준 포트번호가 생략되어 있다.(HTTP의 경우 80, HTTPS의 경우 443)
  - <https://www.google.co.kr:443>
  - <http://www.google.co.kr:80>

4. __Path__

- 파일의 경로
- `/`뒤에 나온다.
  - <https://google.com/search/howsearchworks>

5. __Prameter__

- 쿼리 스트링
- ? 물음표 뒤에 나열되고, &기호로 구분되어 여러개가 존재 할 수 있다.
- ?key=value&key=value

<br/>

### 📖 API란?

- Application Programming Interface
- 서로 다른 어플리케이션(프로그램)이 데이터를 주고 받을 수 있도록 도와주는 매개체(= 일종의 규약)
- __클라이언트와 서버__ 사이의 데이터 전송 통신을 위한 규칙이나 룰,
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

#### 🤖 HTTP Method(CRUD)

- __서버가 수행 해야 할 동작을 지정__ 하여 요청(request)을 보내는 방법
- __주로 사용하는 메서드__ : GET,POST,PUT,DELETE + PATCH
- Read는 Collection(복수)과 Item(Element)(단수)로 나뉨

   1. Create (Collection Pattern 활용) → `POST`/products ⇒ 상품 추가 (JSON 정보 함께 전달)
   2. Read (Collection) → `GET` /products ⇒ 상품 목록 확인
   3. Read (Item) → `GET` /products/{id} ⇒ 특정 상품 정보 확인
   4. Update (Item) → `PUT(덮어쓰기)` 또는 `PATCH(일부 변경)` /products/{id} ⇒ 특정 상품 정보 변경 (JSON 정보 함께 전달)
   5. Delete (Item) → `DELETE` /products/{id} ⇒ 특정 상품 삭제

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

- [URL의 구조](https://velog.io/@liankim/URL의-구조)
- [API란 무엇일까?](https://velog.io/@kwontae1313/API란-무엇일까)
- [REST란? REST API란? RESTful이란?](https://velog.io/@seokkitdo/Network-REST란-REST-API란-RESTful이란)
- [GraphQL이란? (REST api와 차이점)](https://hahahoho5915.tistory.com/63)
- [HTTP Method란?](https://youwjune.tistory.com/42)
- [Rest API vs GraphQL](https://blog.toktokhan.dev/rest-api-vs-graphql-7348f54a220b)

<br/>

### 📖 Fetch API

- 웹 브라우저에서 사용하는 Web API
- 클라이언트(브라우저)측에서 서버로부터 데이터를 요청하기 위해 사용한다.
- 브라우저에 내장된 `fetch()` 함수를 이용한다.

#### 기본적인 사용법 실험

```javascript
fetch('http://localhost:3000/products');
// → Promise
```

fetch 함수는 HTTP response 객체를 래핑한 __Promise__ 객체를 반환한다.

```javascript
fetch('http://localhost:3000/products')
  .then(response => console.log(response));

const response = await fetch('http://localhost:3000/products');
// → Response
```

따라서 프로미스의 후속 처리 메서드인 then(await)을 사용하여 resolve한 객체를 전달받을 수 있다.

```javascript
Response {
  __proto__: Response {
    type: 'basic',
    url: 'http://localhost:3000/products',
    redirected: false,
    status: 200,
    ok: true,
    statusText: '',
    headers: Headers {},
    body: ReadableStream {},
    bodyUsed: false,
    arrayBuffer: ƒ arrayBuffer(),
    blob: ƒ blob(),
    clone: ƒ clone(),
    formData: ƒ formData(),
    json: ƒ json(),
    text: ƒ text(),
    constructor: ƒ Response()
  }
}
```

fetch 함수로 받은 Response 객체에는  HTTP 응답을 나타내는 프로퍼티들이 있다. Response 객체의 body에는`ReadableStream`의 내용이 들어있다.

```javascript
const reader = response.body.getReader();

const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입
// → 원래는 chunk.done이 true일 때까지 반복해야 함

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

.getReader()를 통해 Reader 객체를 얻어 데이터를 읽을 수 있다.
이때 chunk.value는 Uint8Array 타입으로 string으로 바꿔줘야 한다.
TextDecoder는 자바스크립트 문자열로 읽을 수 있는 기능을 제공한다.

```javascript
fetch('http://example.com/movies.json')
  .then((res) => res.json())
  .then((data) => console.log(data));
```

```javascript
const response = await fetch('http://example.com/movies.json'); 
const data = await response.json()
```

 __json() 내장 함수가 있는데__, res.json 메서드 사용 시 HTTP 응답 body 텍스트를 JSON 형식으로 바꾼 프로미스를 반환한다.
(자주 썼던 .then(res ⇒ res.json())의 의미였다)

<br/>

### 📖 [Promoise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)란?

- 비동기 연산이 종료된 이후에 결과 값과 실패 사유를 처리하기 위한 처리기를 연결할 수 있다.
- 프로미스를 사용하면 비동기 메서드에서 마치 __동기 메서드처럼__ 값을 반환할 수 있다.
- 다만 최종 결과를 반환하는 것이 아니고, __미래의 어떤 시점에 결과를 제공하겠다는 '약속'(프로미스)을 반환한다.__

<br/>

### 🔗 참고

- [fetch 함수 쓰는 법, fetch 함수로 HTTP 요청하는 법](https://velog.io/@eunjin/JavaScript-fetch-함수-쓰는-법-fetch-함수로-HTTP-요청하는-법)
