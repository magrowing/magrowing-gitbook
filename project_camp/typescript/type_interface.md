# Type vs Interface

## 학습 키워드

- 타입별칭(Type)
- 인터페이스(interface)
- 타입별칭(Type) vs 인터페이스(interface)

<br/>

## 타입별칭

- 변수를 선언하듯 타입을 별도로 정의할 수 있다.
- `type 타입이름 = 타입` 타입의 이름은 대문자로 지정

```typescript
type TGender = 'female' | 'male';
type TUser = {
  name: string;
  age: number;
  gender: TGender; // 👈🏻 type별칭 안에 또다른 type별칭으로 적용이 가능하다.
};

const user1: TUser = {
  name: '홍길동',
  age: 20,
};
```

> 🚨 동일한 스코프 내에서는 동일한 이름의 타입별칭을 선언하는것은 불가능하다!

```typescript
type TUser = {
  name: string;
  age: number;
};

type TUser = {
  // ❎ 동일한 이름이 타입 지정은 안된다.
};
```

```typescript
type TUser = {
  name: string;
  age: number;
};

function Test() {
  type TUser = {
    // ✅ 스코프가 다르면 동일한 이름 사용 가능
    name: string;
    address: string;
  };

  const user: TUser = {
    name: '홍길동',
    address: '서울특별시',
  };
}
```

> 🚨 타입을 리터럴로 지정해둔다면?

```typescript
type TGender = 'female' | 'male';
type TUserPick = {
  name: string;
  age: number;
  gender?: TGender; // 👈🏻  타입을 'female' | 'male' 라고 리터럴 방식으로 지정
};

const user1 = {
  name: '이나영',
  age: 30,
  gender: 'female', // 👈🏻 타입추론에 의해 타입은 string 지정
};

const user2: TUserPick = user1; // 🚨 gender의 타입이 맞지 않아, error
```

### 타입별칭의 확장

- `&` 연산자를 이용한 확장

```typescript
type Person = {
  name: string;
  age: number;
};

type MyWorker = Person & {
  // & 연산자를 통해 타입 확장
  company: string;
  readonly position: string; // 해당 키워드가 붙게되면 값을 읽어오는건 되지만, 변경하는건 할 수 없다.
  getMoney: (amount: number) => number;
};

type Employ = Person & MyWorker; // 새로운 타입을 생성해서 & 연산자를 사용한 확장법
```

> ✅ 읽기전용 readonly : 해당 키워드가 붙이면 값을 읽는건, 객체의 value 값 변경하는건 할 수 없다.

```typescript
interface Person {
  readonly name: string;
  age?: number;
}

const person: Person = {
  name: '김철수',
  // age: 27,
};

person.name = '홍길동'; // ❌
```

<br/>

## 인터페이스

- 타입 별칭과 동일하게 타입에 이름을 지어주는 또 다른 문법
- 원시타입은 지정할 수 없고, `객체` 타입만 지정 가능
- `interface 타입이름 {}` 타입의 이름은 대문자로 지정

```typescript
interface Person {
  name: string;
  age: number;
}
```

> ⭐️ 동일한 스코프 내에서는 동일한 이름의 인터페이스 선언이 가능하다!

```typescript
interface IBox {
  width: number;
  height: number;
}

interface IBox {
  shape: string;
}

const boxModel2: IBox = {
  width: 20,
  height: 30,
  shape: '네모',
};
```

### 인터페이스의 확장

- `&` 연산자를 활용한 확장

```typescript
interface IPerson {
  name: string;
  age: number;
}

interface IWorker {
  company: string;
  readonly position: string;
  getMoney: (amount: number) => number;
}

const employee: IWorker & IPerson = {
  name: '이나영',
  age: 30,
  company: 'google',
  position: 'teacher',
  getMoney: (amount: number) => {
    return amount;
  },
};
```

- `extends`를 사용한 확장

```typescript
interface IUser {
  name: string;
  age: number;
}

interface IUserAddress extends IUser {
  code: string;
  address: string;
}

const userAddress: IUserAddress = {
  name: '김철수',
  age: 20,
  code: '13456',
  address: '서울특별시 강남구',
};
```

### 다중확장

- `,`를 사용한 확장

```typescript
interface IUser {
  name: string;
  age: number;
}

interface IWorker {
  company: string;
  readonly position: string;
  getMoney: (amount: number) => number;
}

interface IUserAddress extends IUser, IWorker {
  // 타입을 확장할 때 extends를 통해 확장한다. ( , 를 통해 확장 가능 )
  code: string;
  address: string;
}

const userAddress: IUserAddress = {
  name: '김철수',
  age: 20,
  code: '13456',
  address: '서울특별시 강남구',
  company: 'google',
  position: 'teacher',
  getMoney: (amount: number) => {
    return amount;
  },
};
```

<br/>

### 타입별칭(Type) vs 인터페이스(interface)

- 선언방식의 차이
- 확장방식의 차이

> 🤥 개발자의 취향에 따라 사용하는 방식이 다를뿐 두가지의 큰 차이는 없다.

<br/>

## 🔗 참고

- [한입크기로 잘라먹는 타입스크립트](https://ts.winterlood.com/c3003661-2e05-4044-a637-a4c5f1284919)
- [캡틴판교 타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/basic-types.html#any)
