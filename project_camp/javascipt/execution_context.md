# 실행컨텍스트

## 학습 키워드

- 실행컨텍스트
  - 콜스택
- 환경정보
  - 환경레코드(environment Recode)
    - 호이스팅
  - 외부 환경 참조 (outer environment Reference)

<br/>

## 실행 컨텍스트

### 콜스택(Call stack)

- 자바스크립트 코드가 실행되면 생성되는 실행컨텍스트(Execution Context)를 저장하는 자료구조

#### 스택(stack)

- 출입구가 하나인 데이터 구조
- 순서대로 a,b,c 데이터를 넣었다면 꺼낼때(실행될때는) 반대로 c → b → a 순으로 실행 됨
- FILO (First in Last Out)

![Stack & Queue](https://blog.kakaocdn.net/dn/b7quqC/btrxQ4XPNJG/m19vlxw2z8pBUKf9shEeS1/img.png)

#### 컨텍스트

- CS에서 말하는 컨텍스트는 **작업이 중단되고 나중에 동일한 지점에서 계속될 수 있도록 저장해야하는** 작업에 사용되는 최소한의 데이터 집합

> 특정 함수 a를 실행하다가 다른 함수 b호출을 만나 b를 실행하고 다시 a지점으로 돌아와 계속 실행하는 것

### 실행컨텍스트

- 실행할 코드에 제공할 `환경정보`들을 모아놓은 객체 (코드를 실행하기 위해 필요한 정보)
- 매개변수의 이름, 함수, 변수명 등이 선언되면 자바스크립트가 관련된 코드를 실행하는데 필요한
  환경정보들을 수집해 모아둔 객체

#### 🤔 코드의 실행 과정을 파악해보자

```javascript
var a = 1;
function outer() {
  function inner() {
    console.log(a); // undefined
    var a = 3;
  }
  inner();
  console.log(a); // 1
}
outer();
console.log(a); // 1
```

![실행 컨텍스트와 콜 스택](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*daPfZz_hXU7AF2BsBWKn7w.png)

1. 자바스크립트 파일이 열리는 순간 `전역 컨텍스트`가 활성화 된다. → 콜스택에 전역컨텍스트가 쌓인다.

2. `outer` 함수가 호출되면 자바스크립트 엔진은 outer에 대한 환경정보를 수집해서 outer 실행 컨텍스트를 생성한 후 콜스택에 담는다.

3. 전역 컨텍스트와 관련된 코드의 실행은 중단하고, outer 실행 컨텍스트와 관련된 코드들을 순차적으로 실행한다.

4. 다시 `inner` 함수가 호출되면 자바스크립트 엔진은 inner에 대한 환경정보를 수집해서 inner 실행 컨텍스트를 생성한 후 콜스택에 담고 inner 함수 내부 코드를 실행한다. 이때 outer 컨텍스트는 중단된다.

5. inner 내부 함수의 실행이 완료되면 콜스택에서 제거되고 다음 순서에 있는 코드를 실행한다.

> ✅ 컨텍스트가 활성화 된다는 의미는? 코드를 실행하는데 필요한 환경정보들을 수집해서 실행 컨텍스트에 저장한다는 것을 의미

<br/>

### 🤔 실행컨텍스트가 저장되는 환경정보란 무엇일까?

환경정보는 3가지를 의미한다.

#### 1. Variable Environment

- 현재 컨텍스트 내 식별자들의 정보 + 외부환경 정보를 의미

#### 2. Lexical Environment

- Variable Environment을 복사한 후 변경사항을 실시간으로 반영한 정보
- 현재 컨텍스트 내부에 어떤 식별자가 있는지, 외부정보는 어떤것들을 참조하도록 구성돼있는지..

#### 3. This Binding

- 식별자가 바라봐야할 대상 객체를 의미

> 📌 Variable Environment, Lexical Environment는 내부는 두가지로 이루어져있다. <br/>
> environment Record (환경레코드) + outer environment Reference

<br/>

### environment Record와 호이스팅

- 환경레코드는 현재 컨텍스트와 관련된 식별자 정보들이 저장된다.
- 환경레코드는 매개변수의이름과 선언된 함수, 그리고 변수명등의 식별자에 어떤값이 할당될 것인지에 대해 관심을 가지지 않으며, 식별자에만 관심을 갖는다. 컨텍스트 내부를 순서대로 훓으며 수집하기 때문에 자바스크립트는 코드 실행전부터 해당 환경에 속한 식별자들을 모두 알고 있게 된다.

#### 호이스팅

> 📖 hoisting : 코드가 최상단으로 끌어올려지는 것 같은 현상을 의미

environment Record가 변수정보를 수집하는 과정을 이해하기 쉬운방법으로 추상화한 개념이다. 실제로 자바스크립트 엔진이 끌어올리지는 않는다.

#### var, let, const 차이

- var 키워드는 변수의 선언부가 호이스팅 되고, 동시에 undefined로 초기화 된다. 그렇기 때문에 선언전에 사용해도 참조에러가 발생하지 않는다.

- let, const는 undefined를 할당하지 않는채로 즉, 초기화가 이루어지지 않고 되지 호이스팅만 된다.
  그래서 `ReferenceError: Cannot access ~~~ before initialization` 이런 에러가 뜬다.

#### 함수선언과 함수표현식의 차이

- 식별자 중에서 함수 선언은 함수 전체가 호이스팅이 되기 때문에 아래의 코드는 실행된다.

```javascript
test(); // 테스트입니다.

function test() {
  console.log('테스트입니다.');
}
```

함수 선언식은 전체가 호이스팅 되지만, 함수 표현식은 변수에 함수를 할당하는 것이므로 변수만 호이스팅 된다.
따라서 함수 표현식은 변수만 호이스팅 되고, 선언된 함수는 할당되기 전이기 때문에 `is not a function` 이라는 에러가 발생한다.

<br/>

## 🔗 참고

- [Execution Context](https://www.nextree.io/execution-context/)
- [Javascript의 콜스택과 이벤트루프](https://frontj.com/entry/8-Javascript%EC%9D%98-%EC%BD%9C-%EC%8A%A4%ED%83%9D%EA%B3%BC-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84)
- [코어자바스크립트 - 실행 컨텍스트](https://emewjin.github.io/core-javascript/2/#%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80)
- [자바스크립트 실행 컨텍스트](https://medium.com/humanscape-tech/자바스크립트-실행-컨텍스트-1302cf139d25)
- [자바스크립트 실행컨텍스트 설명 - 코드로 살펴보기](https://youtu.be/X4yfufBxq50?si=f6W445H1RIi58cbu)
