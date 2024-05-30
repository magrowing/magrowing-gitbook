# 유틸리티

## 학습 키워드

- 유틸리티
  - Partial
  - Required
  - Pick
  - Omit
  - Record

<br/>

## 유틸리티

- 타입스크립트가 자체적으로 제공하는 특수한 타입들

### Partial

- 특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 변환
- 기존 객체 타입에 정의된 프로퍼티들 중 `일부분만 사용할 수 있도록 도와주는 타입`

```typescript
type TPerson = {
  name: string;
  age: number;
  address: string;
};

const person: Partial<TPerson> = {
  name: '홍길동',
};
```

### Required

- 특정 객체 타입의 모든 프로퍼티를 필수(선택적이지 않은) 프로퍼티로 변환
- 기존 객체 타입에 정의된 프로퍼티들 중 `모든 속성을 필수로 사용해야하는 타입`

```typescript
type TCar = {
  make?: string;
  model?: string;
  year?: number;
};
const car1: Required<TCar> = {
  make: 'Toyota',
  model: 'Corolla',
  year: 2015,
};
```

### Pick

- 특정 객체 타입으로부터 `특정 프로퍼티 만을 골라내는 타입`

```typescript
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const legacyPost: Pick<Post, 'title' | 'content'> = {
  title: '',
  content: '',
};
```

### Omit

- 특정 객체 타입으로부터 `특정 프로퍼티 만을 제거하는 타입`

```typescript
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const noTitlePost: Omit<Post, 'title'> = {
  content: '',
  tags: [],
  thumbnailURL: '',
};
```

### [Record](https://www.typescriptlang.org/docs/handbook/utility-types.html#recordkeys-type)

- 속성 키가 Keys이고 속성 값이 Type인 객체 유형
- 속성을 다른 유형에 매핑하는 데 사용할 수 있는 타입

```typescript
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = 'miffy' | 'boris' | 'mordred';

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: 'Persian' },
  boris: { age: 5, breed: 'Maine Coon' },
  mordred: { age: 16, breed: 'British Shorthair' },
};
```

<br/>

## 🔗 참고

- [한입크기로 잘라먹는 타입스크립트](https://ts.winterlood.com/c3003661-2e05-4044-a637-a4c5f1284919)
