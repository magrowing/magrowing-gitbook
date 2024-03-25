# Axios

## 학습 키워드

- Axios
  - Ajax
  - Fetch vs Axios
- 사용법

<br/>

## [Axios](https://axios-http.com/kr/docs/intro)

- Node.js와 브라우저를 위한 Promise 기반 HTTP 클라이언트 (HTTP 비동기 통신 클라이언트)

### 특징

- 브라우저를 위해 XMLHttpRequests 생성
- node.js를 위해 http 요청 생성
- Promise API를 지원
- 요청 및 응답 인터셉트
- 요청 및 응답 데이터 변환
- 요청 취소
- ✅ JSON 데이터 자동 변환
- XSRF를 막기위한 클라이언트 사이드 지원

<br/>

### 🤔 왜 Axios를 사용하는 이유?

우선 React는 비동기 통신을 위한 HTTP Client를 구현하기 위한 기능이 제공 되지 않는다.
따라서 React Ajax를 구현하기 위해서는 JavaScript 내장 객체인 XMLRequest를 사용하거나 , 다른 HTTP Client를 사용 해야 한다.

브라우저가 제공하는 Fetch API를 사용해도 되지만 보다 더 많은 기능을 제공해서 사람들의 선호도가 높아 많이 사용된다.

<br/>

#### 📖 Ajax

- 브라우저가 갖고 있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로고침 하지 않아도 페이지의 일부분만을 위한 데이터를 로드하는 기법
- JavaScript를 사용한 비동기 통신, 클라이언트와 서버 간에 XML 데이터를 주고 받는 기술

<br/>

### Fetch vs Axios

||Fetch|Axios|
|---|:---:|:---:|
|유형|브라우저 빌트인|써드파티 라이브러리|
|데이터|data 속성을 사용|body 속성을 사용|
|데이터 타입|data는 object를 포함한다.|body는 문자열화 되어있다.|
|응답|status가 200이고 statusText가 ‘OK’이면 성공이다.|응답객체가 ok 속성을 포함하면 성공이다.|
|json|자동으로 JSON데이터 형식으로 변환|.json()메서드를 사용|
|요청 커스텀|요청을 취소할 수 있고 타임아웃을 걸 수 있다.|해당 기능 존재 하지않음|
|HTTP 요청|HTTP 요청을 가로챌수 있음|기본적으로 제공하지 않음|
|download|download 진행에 대해 기본적인 지원을 함|지원하지 않음|
|지원 브라우저|좀 더 많은 브라우저에 지원됨|Chrome 42+, Firefox 39+, Edge 14+, and Safari 10.1+|

<br/>

### ⚙️ 설치 및 사용법

```shell
npm install axios
```

#### `get` 요청

```jsx
// Promise chaining
axios
  .get('/user', {params : { ID : 12345 },
  })
  .then((response) => console.log(response))
  .catch((error) => console.log(error) )
  .then(() => console.log('done'));
```

```jsx
// async await
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    return response.data;
  } catch (error) {
    console.log(error);
  }
}
```

#### `post` 요청

```jsx
// Promise chaining
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

```jsx
// async await
async function postUser() {
  try {
    const response = await axios.post('/user',  {
    firstName: 'Fred',
    lastName: 'Flintstone'
  });
  return response.data;

  } catch (error) {
    console.log(error);
  }
}
```

<br/>

### 🔗 참고

- [fetch와 axios 차이점과 비교](https://tlsdnjs12.tistory.com/26)
- [axios 개념, Fetch API와의 차이점 (+axios 사용하기)](https://eundol1113.tistory.com/256)
- [Axios - HTTP 비동기 통신 라이브러리](https://velog.io/@sunohvoiin/Axios-HTTP-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC)
