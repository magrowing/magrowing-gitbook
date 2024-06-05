# Server

## 학습 키워드

- Express 이용한 간단한 Server 구축
- Fetch API
- Axios

<br/>

## Express 이용한 간단한 Server 구축

- cors
- express
- nodemon
- uuid

```shell
npm i cors express nodemon uuid
```

### Express의 Hello world 예제 사용

- server.js
- port : 4000

### server 구동 방법

- 해당 방식은 수정사항을 반영해주지 않아, server off 하고 재구동

```shell
node server.js // 👈🏻 구동할 파일
```

- nodemon은 server.js의 수정사항을 감지해서 바로 반영해주기 때문에 사용

```shell
npx nodemon server.js
```

#### 📚 API를 사용하기 위해서 알아야 하는 개념

- 웹브라우저와 서버의 통신 하기위해서 HTTP 프로토콜을 사용
- HTTP Protocol : TCP/IP 기반으로 클라이언트와 서버 사이에 이루어지는 요청/응답 프로토콜
- TCP/IP 기반으로 클라이언트와 서버 사이에 이루어지는 요청/응답 프로토콜
- HTTP : TCP/IP
- QUIC : UDP 기반으로 빠른 전송 속도를 가지는 프로토콜
- HTTP Method : GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD, TRACE
  - GET : 데이터를 요청하기 위해 사용되는 메소드 → 보안의 약함
  - POST : 데이터를 생성하기 위해 사용되는 메소드 → 보안상 안정성 높음
  - PUT : 데이터를 업데이트하기 위해 사용되는 메소드 → 데이터를 `전체적으로` 업데이트
  - PATCH : 데이터를 업데이트하기 위해 사용되는 메소드 → 데이터를 `일부분만` 업데이트
  - DELETE : 데이터를 삭제하기 위해 사용되는 메소드
- Http/Https : Https(보안 인증서가 추가된 프로토콜) → 인프라 영역

> 🤔 RESTful, REST 호출한다라는 의미는? HTTP Method를 규칙에 맞는 API 설계 전략

<br/>

## Fetch API

### CORS

- 해결하는 주체는 Backend (근본)

> 🚨 Access to fetch at '<http://localhost:4000/todos>' from origin '<http://localhost:5173>' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.

#### 학습을 목적임으로, Express 미들웨어로 해결

```shell
npm i cors
```

```javascript
// server.js
const cors = require('cors');

app.use(cors());
```

### GET

- 기본 사용법

```jsx
const fetchDate = () => {
  fetch('http://localhost:4000/todos') // 👈🏻 Promise 반환
    .then((res) => res.json())
    .then((data) => console.log(data))
    .catch((e) => console.log(e));
};
```

- async/await 과 함께 사용하는 방법

```jsx
const fetchDateAsync = async () => {
  try {
    const response = await fetch('http://localhost:4000/todos');
    const data = await response.json();
    console.log(data);
  } catch (e) {
    console.log(e);
  }
};
```

```jsx
const fetchDateAsync = async () => {
  try {
    const data = await (await fetch('http://localhost:4000/todos')).json();
    console.log(data);
  } catch (e) {
    console.log(e);
  }
};
```

### POST

```jsx
const fetchAdd = async () => {
  await fetch('http://localhost:4000/todos', {
    method: 'POST', // 👈🏻 항상 대문자로
    headers: {
      'Content-Type': 'application/json', // 👈🏻 꼭 Data의 타입을 명시해주어야 한다.
    },
    body: JSON.stringify({ text: 'Learn React' }),
  });
};
```

### PATCH

```jsx
const fetchToggle = async () => {
  await fetch(`http://localhost:4000/todos/${id}`, {
    method: 'PATCH',
  });
};
```

### DELETE

```jsx
const fetchDelete = async () => {
  await fetch(`http://localhost:4000/todos/${id}`, {
    method: 'DELETE',
  });
};
```

<br/>

## Axios

- 설치

```shell
npm i axios
```

> 🧐 면접질문 : 둘다 http 리퀘스트를 만드는건 동일하다. fetch API 내장 함수이고, axios는 라이브러리이기 때문에 fetch API 보다 더 넓은 범위를 커버 할 수 있다. (사용에 있어 편의성을 제공하는 장점이 있다.)

### GET

```jsx
const axiosDate = () => {
  axios
    .get('http://localhost:4000/todos')
    .then((res) => {
      console.log(res);
      console.log(res.data);
    })
    .catch((e) => console.log(e));
};
```

```jsx
const axiosDateAsync = async () => {
  try {
    const response = await axios.get('http://localhost:4000/todos');
    console.log(response.data);
  } catch (e) {
    console.log(e);
  }
};
```

### POST

```jsx
const axiosPost = async () => {
  try {
    await axios.post('http://localhost:4000/todos', {
      text: 'Learn React Axios',
    });
  } catch (e) {
    console.log(e);
  }
};
```

### Patch

```jsx
const axiosPatch = async () => {
  try {
    await axios.patch(`http://localhost:4000/todos/${id}`);
  } catch (e) {
    console.log(e);
  }
};
```

### Delete

```jsx
const axiosDelete = async () => {
  try {
    await axios.delete(`http://localhost:4000/todos/${id}`);
  } catch (e) {
    console.log(e);
  }
};
```
