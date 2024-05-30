# function & Generic

## 학습 키워드

- 함수
- 제네릭

<br/>

## function

### 함수선언식

- 매개변수의 타입을 지정해주어야 한다.
- 함수의 반환값 타입은 `자동으로 추론`되기 때문에 다음과 같이 생략가능하다.

```typescript
function add(x: number, y: number): number {
  return x + y;
}

function minus(x: number, y: number): number {
  return x - y;
}

function multiply(x: number, y: number): number {
  return x * y;
}
```

### 함수표현식

- 함수 선언식과 동일하게 매개변수의 타입을 지정해주어야 한다.

```typescript
const add = function (x: number, y: number) {
  return x + y;
};

const minus = function (x: number, y: number) {
  return x - y;
};

const multiply = function (x: number, y: number) {
  return x * y;
};
```

> 🤔 그런데 함수 표현식은 코드 중복인 부분들이 있는데 줄일수 없을까? <br>
> type 별칭과 인터페이스를 사용해서 중복된 코드를 줄일 수 있다! 그래서 타입스크립트를 사용하는 경우, 함수표현식과 화살표 함수를 선호한다.

```typescript
type TCommonFu = (x: number, y: number) => number;

interface ICommonFu {
  (x: number, y: number): number;
}

const add: TCommonFu = function (x, y) {
  return x + y;
};

const minus: ICommonFu = function (x, y) {
  return x - y;
};

const multiply: ICommonFu = function (x, y) {
  return x * y;
};
```

<br/>

## 제네릭

### 🤔 제네릭이 필요한 상황

함수의 매개변의 댜양한 타입이 지정되어야 한다. 그렇다면 그때 마다 타입을 추가해주어야 하는 것일까?
매개변수가 길어지고 오히려 코드가 복잡해지는데? 그렇다면 any 지정해야하는 걸까? 그것도 아니다!
그래서 등장한 개념이 `제네릭 함수`이다.

```typescript
function getSize(values: number[] | string[] | (number | string)[]): number {
  return values.length;
}

getSize(['1', '2', '3']);
getSize([1, 2, 3]);
getSize(['1', '2', '3', 1, 3, 4]);
```

### 🏥 제네릭 함수

> 📖 제네릭(Generic) : 일반적인 또는 포괄적인 이라는 뜻

- **두루두루 모든 타입의 값을 다 적용할 수는 범용적인 함수로** 이해하면 된다.

```typescript
function func<T>(value: T): T {
  return value;
}

let num = func(10); // number 타입
```

함수 이름 뒤에 `<>` 와 `T` 를 선언한다. 그리고 매개변수와 반환값의 타입을 `T` 로 설정한다.
`T` 에 어떤 타입이 할당될 지는 함수가 호출될 때 결정된다.

func(10) 처럼 `Number` 타입의 값을 인수로 전달하면 매개변수 value에 `Number` 타입의 값이 저장되면서 T가 Number 타입으로 추론된다. 이때 T는 Number 타입으로 추론되고, func 함수의 반환값 타입 또한 Number 타입이 된다.

![출처 : 한입크기로 잘라먹는 타입스크립트 핸드북](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6442ef29-4a0e-4d95-9a68-4b697b80cb59%2FUntitled.png?table=block&id=33d9f7a5-484d-49a7-8c74-bc3ec9930f60&cache=v2)

#### 👩🏻‍💻 해당 코드를 제네릭을 사용해서 수정해보자

```typescript
function getSize(values: number[] | string[] | (number | string)[]): number {
  return values.length;
}
```

```typescript
function getSize<T>(values: T[]): number {
  return values.length;
}

getSize(['1', '2', '3']);
getSize([1, 2, 3]);
getSize(['1', '2', '3', 1, 3, 4]);
```

### 🤹🏻‍♀️ 제네릭의 활용

```typescript
type TUserBase<T> = {
  name: string;
  age: number;
  likeFoods: T; // 명확한 타입을 넣기 어려울때도 제네릭을 이용해서 정의 할 수 있다.
};

interface IUserBase<T> {
  name: string;
  age: number;
  likeFoods: T; // 명확한 타입을 넣기 어려울때도 제네릭을 이용해서 정의 할 수 있다.
}

const personHong: TUserBase<string[]> = {
  name: '홍길동',
  age: 20,
  likeFoods: ['김밥', '피자'],
};

const personKim: TUserBase<null> = {
  name: '김철수',
  age: 27,
  likeFoods: null,
};

const personLee: TUserBase<string> = {
  name: '이나영',
  age: 24,
  likeFoods: '김밥',
};
```

<br/>

## 🔗 참고

- [한입크기로 잘라먹는 타입스크립트](https://ts.winterlood.com/c3003661-2e05-4044-a637-a4c5f1284919)
- [캡틴판교 타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/basic-types.html#any)
