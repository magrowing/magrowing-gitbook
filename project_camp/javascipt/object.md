# 객체

## 학습 키워드

- 객체
- 생성자 함수
  - 프로토타입
  - Wrapper Object
- Class

<br/>

## 객체

- 데이터를 하나로 관리해주는 데이터 형태
- 배열과의 차이점이 있다면 객체는 `key-value` 존재한다.

### 객체 프로퍼티 (객체 속성)

- key : value
- value : 문자열, 숫자, boolean, 함수, 객체, 배열 타입 사용 가능

```javascript
let objValue = {
  str: 'string', // 👈🏻 객체 프로퍼티 key : value
  num: 27,
  bol: true,
  obj: {},
  arr: [],
  func: function () {},
};
```

### 객체 프로퍼티를 다루는 방법

#### 1. 특정 프로퍼티에 접근하는 방법

- 점 표기법

```javascript
let str = objValue.str;
console.log(str); // string
```

- 괄호 표기법 `['문자열']`
  - `''` 없으면 변수로 인식해서 Error
  - 동적으로 생성시 유용

```javascript
let num = objValue['num'];
console.log(num); // 27
```

#### 2. 새로운 프로퍼티 추가하는 방법

```javascript
objValue.newType1 = '점 표기법으로 생성';
objValue['newType2'] = '괄호 표기법으로 생성';
```

#### 3.프로퍼티를 삭제하는 방법 `delete`

```javascript
delete objValue.newType1;
delete objValue['newType2'];
console.log(objValue);
```

### 객체 반복문

- 객체 데이터를 하나씩 접근하기 위한 반복문 `for-in`

```javascript
let obj = { name: '철수', age: 20 };
for (let key in obj) {
  console.log(obj[key]);
}
```

### 상수 객체

- 상수로 키워드로 생성한 객체의 프로퍼티는 수정이 가능하다.
  - 실제로 객체의 값을 저장하는게 아니라 (메모리)주소를 저장하기 때문에

```javascript
const obj = { name: '철수', age: 20 };

obj.year = 2005;
console.log(obj); // { name: '철수', age: 20, year : 2005 }
```

### 메서드

- 객체의 value 값으로 사용되는 함수 → 다른 용어로 메서드(method)

```javascript
const obj = {
  speak: function () {
    console.log('객체 입니다.');
  },
};

// ✅ 단축 문법 : ES6문법 변경 사항
const obj = {
  speak() {
    console.log('객체 입니다.');
  },
};
```

<br/>

## 생성자 함수

- 객체를 생성하는 함수
- 유사한 객체를 여러개 만들어야 할 때 경우 사용
  - 함수의 이름은 첫글자는 `대문자`로 시작
  - `new` 연사나를 붙여 함께 호출 인스턴스 객체 생성
  - 함수의 매개변수를 활용 할 수 있다는 특징

```javascript
function User(name) {
  // this = {}; (빈객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this; (this가 암시적으로 반환)
}

let user = new User('보라'); // 인스턴스 객체

console.log(user.name); // 보라
console.log(user.isAdmin); // false
```

<br/>

### 프로토타입(prototype)

- 생성자 함수를 만들게 되면 1:1로 매칭되는 함수 멤버로 prototype 속성이 있다.
- `prototype` 객체의 멤버인 `constructor` 속성은 함수를 참조하는 내부구조를 가진다.

<br/>

### Wrapper Object

- 래퍼 객체란 이름처럼 원시 타입의 값을 감싸는 형태의 객체이다. number, string, boolean, symbol 데이터 타입에 각각 대응하는 Number, String, Boolean, Symbol이 제공된다.

```javascript
const str = 'apple';

console.log(str.length); // 5
```

#### 🤔 해당 코드가 실행되는 원리는?

해당 코드를 실행하면 결과값은 5가 나온다. 여기서 이상한 점이 발견된다.
str은 문자열인 원시타입이다. 객체의 프로퍼티를 접근가능하는 `.length`를 어떻게 사용할 수 있는 것일까?

> 자바스크립트의 문자열은 원시타입으로 존재한다. 우리가 문자열의 프로퍼티에 접근하려고 할 때, 자바스크립트는 `new String`을 호출한 것처럼 문자열 값을 객체로 변환한다. 이 객체를 `래퍼객체` 라고 한다. 래퍼 객체는 프로퍼티를 참조할 때 생성되며 프로퍼티 참조가 끝나면 사라진다.

```javascript
const str = 'apple';

// 👇🏻 해당 코드를 실행하기 위해
console.log(str.length); // const str = new String('apple') → 일시적으로 객체로 변환
```

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

### 상속

- `extends` 키워드를 사용해서 상속 받을 수 있다.
- 상속을 받는다면 `constructor()` 내부에 `super()` 함수가 존재해야 한다.

```javascript
class Shape {
  // 상속을 하든, 상속을 하지 않든, 반드시 constructor를 가지고 있어야 한다.
  constructor(color) {
    this.color = color;
  }

  getColor() {
    return this.color;
  }
}

class Rectangle extends Shape {
  constructor(color, width, height) {
    super(color); // 상속받는다면 무조건 super 함수 있어야함.
    this.width = width;
    this.height = height;
  }
  getArea() {
    return this.width * this.height;
  }
  getColor() {
    return `사각형의 색상은 ${this.color}`; // 👈🏻 오버라이딩
  }
}
```

> 📖 메서드 오버라이딩 <br/> 상속한 상위 클래스의 메서드를 수정하고 싶거나 더 확장하기 위해 상속 받은 class 내에서 메서드를 재정의하는 행위

### get & set

- `set` : 값을 설정하는 키워드
- `get` : 값을 가져오는 키워드

```javascript
class Car {
  constructor(speed) {
    this.speed = speed;
  }

  get speed() {
    return this._speed;
  }

  set speed(value) {
    this._speed = value < 0 ? 0 : value;
  }

  getSpeed() {
    return this.speed;
  }

  // 가정... 수많은 함수들이 구현되어 있다.
  // 가정... 그 수많은 함수에서 speed를 사용하고 있다.
}
const car1 = new Car(-100);
console.log(car1.getSpeed());
```

### private

- 2020년 최신 문법
- 순수 웹브라우저에서는 `#private` 속성은 get&set를 제어 할 수 없다. (컴파일러 사용 해야한다.)
- 외부에서 값의 변경을 막고자 할 때 private 속성을 사용한다.

```javascript
class Car {
  #speed;
  constructor(speed) {
    this.#speed = speed;
  }
  getSpeed() {
    return this.#speed;
  }
}
const car1 = new Car(100);
console.log(car1.getSpeed());
// car1.#speed = 100; // 🚨 바꿀수 없다는 error 출력
```

### static

- `static`으로 정의된 것들은 인스턴스화 되지 않는다.

```javascript
class MathUtils {
  static APP_NAME = 'Math Utils';
  static PI = 3.14;
  constructor(number) {
    this.number = number;
  }

  static add(a, b) {
    return a + b;
  }
}
const mathUtils = new MathUtils(10);
console.dir(mathUtils);

console.log(MathUtils.PI); // 👈🏻 static은 인스턴스화 되지 않아서 생성자 함수 식별자를 사용해서 접근한다.
console.log(MathUtils.add(2, 4));
```

<br/>

## 🔗 참고

- [new 연산자와 생성자 함수](https://ko.javascript.info/constructor-new)
- [자바스크립트 : prototype 이해](https://www.nextree.co.kr/p7323/)
- [wrapper object](https://velog.io/@kim-jaemin420/Wrapper-Object래퍼-객체-jyt19oms)
- [래퍼 객체 (wrapper object)](https://hwb0218.tistory.com/46)
- [클래스와 기본 문법](https://ko.javascript.info/class)
- [클래스 상속](https://ko.javascript.info/class-inheritance#ref-32)
