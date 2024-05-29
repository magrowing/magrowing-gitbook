# DOM

## 학습 키워드

- DOM
  - 문서객체 선택
  - 문서객체 조작
  - Node
  - 이벤트

<br/>

## DOM

- Document Object Model, 문서 객체 모델
- 웹 브라우저에 표시되는 HTML 태그를 자바스크립트가 이해하기 위해 객체로 변환된 형태를 말한다.
- 웹 브라우저에 표시되는 HTML 태그를 자바스크립트 객체로 변환하고 조작할 수 있도록 API를 제공하는 역활을 담당한다.

> ⭐️ **DOM의 핵심** <br/> 문서 객체를 선택하고 제어 하는 것이다.

### 문서객체 선택

- `id` 속성 (단일)

```javascript
document.getElementById(id);
```

- `class 이름` 속성 (복수)

```javascript
document.getElementsByClassName(className);
```

- `태그 이름` 사용 (복수)

```javascript
document.getElementsByTagName(name);
```

- ⭐️ `선택자` 활용
  - 권장하는 방식
  - 선택자는 태그이름, 클래스, id속성 전부 가능하다.

```javascript
document.querySelector(selector); //단일
document.querySelectorAll(selector); //복수
```

### 문서객체 조작

#### 1. 스타일 변경 : `style`

```javascript
document.querySelector('p').style.color = 'red';
document.querySelector('p').style.display = 'none';
```

#### 2. 속성추가 : `setAttribute`

```javascript
document.querySelector('p').setAttribute('class', 'text');
```

#### 3. 속성제거 : `removeAttribute`

```javascript
document.querySelector('p').removeAttribute('class');
```

#### 2. 속성제거 : `removeAttribute`

```javascript
document.querySelector('p').removeAttribute('class');
```
