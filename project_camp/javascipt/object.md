# 객체

## 학습 키워드

- 객체
- 생성자 함수
  - 프로토타입
- Class

<br/>

## Class

> 📖 Class <br> 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀로, 객체를 정의 하기 위한 상태(멤버변수)와 메서드(함수)로 구성된다.

자바스크립트는 `class` 에 대한 개념이 있지 않았다. 따라서 생성자 함수를 사용해서 정의하곤 했다.
ES6에서 `class`문법이 도입되면서 객체 지향 프로그맹에서 사용되는 다양한 기능을 자바스크립트에서도 사용할 수 있다.

### 기본문법

- class 키워드를 사용
- `constructor()` : 기본 상태를 설정해주는 생성자 메서드 new에 의해 자동으로 호출 된다.

```javascript
class User {
  // 꼭 해당 메서드가 존재해야 한다.
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return `Hello! ${this.name}`;
  }
}

const user = new User('홍길동');
user.sayHi();
```

<br/>

## 🔗 참고

- [클래스와 기본 문법](https://ko.javascript.info/class)
