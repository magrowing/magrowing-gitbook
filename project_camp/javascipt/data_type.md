# 자료형(DataType)

## 학습 키워드

- 자료형(DataType)
  - 원시 자료형 (Primitive DataType)
  - 참조 자료형 (Reference DataType)
  - 데이터 자료형 확인 typeof

<br/>

## 자료형(DataType)

- 자바스크립트에서의 값은 특정한 자료형에 속하게 된다.

### 원시 자료형 (Primitive DataType)

#### Number

- 정수, 부동 소수점 숫자 등의 숫자를 나타낼 때 사용
- `Infinity`, `-Infinity`,`NaN` 포함

#### String

- 빈 문자열이나 글자들로 이뤄진 문자열을 나타낼 때 사용

#### Boolean

- true / false

#### Null

- null 값만을 위한 독립 자료형
- 개발자가 의도적으로 값이 없을 경우 사용

#### Undefined

- undefined 값만을 위한 독립 자료형

#### [Symbol](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

- 고유함이 보장되는 자료형
- 객체에 속성을 추가할 때 고유한 키를 부여하여 다른 코드와 충돌하지 않도록 할 때 사용

<br/>

### 참조 자료형 (Reference DataType)

- 객체(Object, Array, Function)

<br/>

### [Typeof](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/typeof)

- 자료형을 나타내는 문자열을 반환하는 연산자

```javascript
console.log(typeof '문자열'); // string
console.log(typeof 20); // number
console.log(typeof undefined); // undefined
console.log(typeof null); // object (자바스크립트의 오류)
console.log(typeof []); // object  (자바스크립트의 오류)
console.log(typeof {}); // object
console.log(typeof function Fun() {}); //function
```

<br/>

### 🤔 DataType이 필요한 이유는 무엇일까?

- 메모리 공간 확보 및 값을 참조하기 위해서 필요

컴퓨터의 메모리의 공간은 유한한다. 값을 저장하려면 메모리의 공간 크기를 결정 해야하고, 값을 읽기 위해서는 메모리의 공간의 크기를 알아야하기 때문에 데이터 타입을 통해 메모리 공간의 크기를 결정하고 확인한다.

<br/>

## 🔗 참고

- [모던 자바스크립트 Deep Dive 6장 : 데이터 타입](https://devysi0827.tistory.com/54)
- [코어자바스크립트 - 자바스크립트의 데이터 타입](https://emewjin.github.io/core-javascript/1/)
