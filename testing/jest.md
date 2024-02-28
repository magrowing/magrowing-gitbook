# Jest

## 학습 키워드

- 테스트 케이스 정의 하는 방법
  - BDD
- Jest
  - 설치 및 실행 방법
  - 기본 사용법  
  - Describe - Context - It 패턴

<br/>

### 🤖 테스트 케이스 정의 하는 방법

#### 1. test 함수로 개별 테스트를 나열하는 방식

```javascript
test('add', () => {
 expect(add(1, 2)).toBe(3);
});
```

#### 2. BDD 스타일로 주체-행위 중심으로 테스트를 조직화하는 방식

```javascript
describe('add', () => { // 주체 : add 함수 
  it('returns sum of two numbers', () => { // 행위 : add 함수가 두 숫자를 더한 값을 반환한다.
    expect(add(1, 2)).toBe(3);
  });
});
```

### 🤔 BDD 스타일은 뭘까?

- BDD(Behavior-driven development)
- 소프트웨어 개발 방법론
- 비즈니스 요구사항에 집중하여 테스트 케이스를 개발한다는 것

<br/>

## [Jest](https://jestjs.io/)

- Facebook이 개발하여 오픈 소스로 공개한 __JavaScript 테스팅 프레임워크__
- 설정이 거의 필요 없는 테스트 프레임워크

### 🛠️ Jest 설치 및 실행 방법

#### 1. Jest 설치

``` shell
# 1.Jest 설치
npm i -D jest @types/jest @swc/core @swc/jest \
  jest-environment-jsdom \
  @testing-library/react @testing-library/jest-dom@5.16.4
```

#### 2. Jest에서 TypeScript 사용하도록 `jest.config.js` 파일 작성

```javascript
module.exports = {
 testEnvironment: 'jsdom',
 setupFilesAfterEnv: [
  '@testing-library/jest-dom/extend-expect',
 ],
 transform: {
  '^.+\\.(t|j)sx?$': ['@swc/jest', {
   jsc: {
    parser: {
     syntax: 'typescript', 
     jsx: true,
     decorators: true,
    },
    transform: { 
     react: {
      runtime: 'automatic',
     },
    },
   },
  }],
 },
};
```

#### 3. Jest 실행

```shell
# jest 실행
npx jest

#jest 자동 실행
npx jest --watchAll
```

<br/>

### ⚙️ Jest 기본 사용 방법

```ts
import { sum } from './sum';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

1. `test` 함수는 첫번째 인자로 이 테스트가 어떤 것을 설명하는지 제목을 입력한다.
2. 두번째 인자로는 콜백함수를 가지는데, 이 안에서 테스트를 할 코드를 작성한다.
3. 콜백함수 안에 있는 `expect`는 __return값을 가지는 어떤 함수를__ 실행한다.
4. __expect된 객체를__ `toBe`를 사용해서 어떤 값이 나올지 인자로 담는다.

> `test` 대신 `it`을 사용할 수 있다. [Also under the alias](https://jestjs.io/docs/api#testname-fn-timeout)

### 🎯 Matchers로 다양한 결과 예측하기

#### `.toBe`

`toBe`의 경우 Object.is를 내부적으로 사용해서 비교하는 두 객체를 비교했을 때 ===이 성립하는지로 이해하면 될 것 같다.

#### `.toEqual`

```jsx
test('object assignment', () => {
  const data = {one: 1};
  data['two'] = 2;
  expect(data).toEqual({one: 1, two: 2});
});
```

`toEqual`은 배열 또는 객체의 값(주소 말고!)이 같은지 확인한다.

> 이외에도 다양한 Matchers들이 존재한다.

### 🧩 Describe-Context-It (Describe-It) 패턴

`describe` 키워드를 사용해서 작은 단위의 테스트 코드를 __그룹화할 수 있다.__

#### 🤔 그룹화 한다는게 무슨 의미 일까?

특정 component에 속하는 테스트 코드라면 `describe` 키워드를 사용해서 해당 테스트 코드들을 그룹화하는 것이 좋다. 이렇게 그룹화를 해주게 되면 나중에 테스트 결과를 확인시 더 가시적으로 보기 편하게 테스트 케이스들을 확인 할 수 있다.

<br/>

### Jest를 이용한 간단한 TDD 예제

#### 📄 파일명 : `파일이름.test.확장자`

- sample.test.ts
- sample.spec.ts (주로 BDD 스타일로 사용 시)

#### 🤖 test 함수로 개별 테스트를 나열하는 방식

- 실패 케이스로 우선 add 함수 작성

```ts
function add(x: number, y: number): number {
 return 0;
}

test('add', () => {
 expect(add(1, 2)).toBe(3);
});
```

- 테스트가 통과 될 수 있도록 add 함수 수정

```ts
function add(x: number, y: number): number {
 return 3;
}

test('add', () => {
 expect(add(1, 2)).toBe(3);
});
```

- 반복적으로 코드를 테스트 하면서 중복제거

```ts
function add(x: number, y: number): number {
 // return 0
 // return 3
 // return 1 + 2; // 의도를 나타냄으로써 중복 제거
 return x + y;
}

test('add', () => {
 expect(add(1, 2)).toBe(3);
});
```

#### 🤖 BDD 스타일로 주체-행위 중심으로 테스트를 조직화하는 방식

- BDD 스타일로 테스트 대상과 행위를 명확히 드러내자.

```ts
function add(x: number, y: number): number {
 return x + y;
}

test('add 테스트', () => {
 expect(add(1, 2)).toBe(3);
});

describe('add', () => { // 대상 
 it('returns sum of two numbers', () => { // 행위 
  expect(add(1, 2)).toBe(3);
 });
});

// 이정는 BDD보다는 함수로 개별 테스트 진행
```

#### 🤔 다양한 경우가 고려 되어야 한다면? ⇒ Describe-Context-It 패턴

```ts
function add(...number: number[]): number {
 // Return number[0] + (number[1] || 0);
 // return (number[0] ?? 0) + (number[1] ?? 0) + (number[2] ?? 0); //코드의 반복이 보여짐

 // return (number[number.length - 3] ?? 0) + (number[number.length - 2] ?? 0) + (number[number.length - 1] ?? 0);

 if (number.length === 0) {
  return 0;
 }

 // If (number.length === 1) {
 //  return number[0];
 // }

 // If (number.length === 2) {
 //  return number[0] + number[1];
 // }

 // If (number.length === 3) {
 //  return number[0] + number[1] + number[2];
 // }

 // if (number.length >= 2) {
 //  return add(number[0] + number[1]) + number[2];
 // }

 // if (number.length >= 2) {
 //  return add(...number.slice(0, number.length - 1)) + number[number.length - 1];
 // }

 // return add(...number.slice(0, number.length - 1)) + number[number.length - 1];

 return number.reduce((acc, current) => acc + current);
}

const context = describe;

test('add 테스트', () => {
 expect(add(1, 2)).toBe(3);
});

describe('add', () => {
 context('with no argument', () => {
  it('returns zero', () => {
   expect(add()).toBe(0);
  });
 });

 context('with only one number', () => {
  it('returns the same numbers', () => {
   expect(add(2)).toBe(2);
  });
 });

 context('with two numbers', () => {
  it('returns sum of two numbers', () => {
   expect(add(1, 2)).toBe(3);
  });
 });
  // 테스트 코드들이 추가 될 때 마다 add 함수 로직 변경 
 context('with three numbers', () => {
  it('returns sum of three numbers', () => {
   expect(add(1, 2, 3)).toBe(6);
  });
 });

 context('with ten numbers', () => {
  it('returns sum of ten numbers', () => {
   expect(add(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)).toBe(55);
  });
 });
});
```

<br/>

### 🔗 참고

- [BDD와 TDD의 차이](https://blog.aliencube.org/ko/2014/04/02/differences-between-bdd-and-tdd/)
- [Jest의 test, it, describe](https://leehyungi0622.github.io/2021/02/17/202102/210217-JS_Jest_test_it/)
- ⭐️ [Jest: 기본기 & Matchers](https://woonjangahn.gitbook.io/logs/typescript/testing/jest-and-matchers)
- [Jest 활용 경험: 테스팅에 대한 고민과 통찰](https://blog.imqa.io/testing-framework-jest/)
