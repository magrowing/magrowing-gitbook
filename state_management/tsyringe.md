# TSyringe

## 학습키워드

- 의존성 주입(Dependency Injection)
- TSyringe
  - reflect-metadata
  - singleton (싱글톤)

<br/>

## 의존성 주입([Dependency Injection](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85))

- 하나의 객체가 다른 객체의 의존성을 제공하는 테크닉
- 의존성 주입의 의도는 `객체의 생성`과 `사용의 관심을 분리하는 것` ⇒ 이는 가독성과 코드 재사용을 높혀준다.

#### 예제를 통해 이해해보기

- TypeScript로 API call을 하는 로직

```jsx
import axios from 'axios';

export const fetchTodo = (todoId: number) => {
  return axios.get<Todo>(`/todos`, { params: { todoId } });
};
```

> fetchTodo 함수는 axios 를 import 해서 이용한다. 즉, fetchTodo 함수는 axios 모듈에 `의존`한다.

### 🤔 그렇다면 의존성 주입은?

#### React의 Context API 의존성 주입 도구

```jsx
// @/contexts/MyContext.ts
export const MyContext = createContext<MyContextType>({});


// 어딘가에 있는 부모 컴포넌트
const Parent = () => {
  return (
    <MyContext.Provider value={{ age: 22 }}>
      ...
    </MyContext.Provider>
  );
};


// 어딘가에 있는 자식 컴포넌트
const Child = () => {
  const { age } = useContext(MyContext);
  
  return <div>나이: {age}</div>;
}; 
```

> Context API는 Provider 를 통해 값을 주입해 주면, useContext 를 통해 해당 값을 구독하는 형태를 가지게 된다. Child 는 age 라는 값을 import 하지 않았고, Context API 를 통해 주입받았다.

<br/>

## TSyringe

> A lightweight dependency injection container for TypeScript/JavaScript for constructor injection.

- TypeScript용 DI(의존성 주입) 도구

<br/>

### 🧐 TSyringe 학습하는 이유는?

TSyringe를 사용해서 `Prop Drilling`문제를 Context를 사용하지 않고 TSyringe를 사용한 해결 방법을 학습

- [Prop Drilling 이란](https://magrowing.gitbook.io/magrowing-gitbook/category/react/props#prop-drilling)

<br/>

### 👩🏻‍💻 간단한 실습을 통해 TSyringe 사용해보기

#### 1. ⚙️ TSyringe, reflect-metadata 설치

- [TSyringe](https://github.com/microsoft/tsyringe)
- [reflect-metadata](https://github.com/rbuckton/reflect-metadata)

```shell
npm i tsyringe reflect-metadata
```

#### 2. 싱글톤으로 관리할 CounterStore Class를 준비

```shell
mkdir src/stores
touch src/stores/CounterStore.ts
```

```ts
// src/stores/CounterStore.ts
import { singleton } from 'tsyringe';

@singleton()
export default class CounterStore{
  count = 0
}
```

#### 3. 📁 src/`main.tsx` 와 src/`setupTests.ts` reflect-metadata import

> 🚨 tsyringe requires a reflect polyfill. Please add 'import "reflect-metadata"' to the top of your entry point.

```
├── package.json
├── package-lock.json
├── src
│   ├── main.tsx ✅
│   ├── App.tsx
│   ├── App.test.tsx 
│   ├── setupTests.ts ✅ 
│   ├── hooks 📁
│   │   ├── useForceUpdate.ts 
│   ├── components 📁
│   │   ├── Counter.tsx 
```

```ts
import 'reflect-metadata';
```

해당 App의 시작 지점(진입지점) main.tsx `reflect-metadata`를 import해줘야 한다.
테스트를 위해서는 setupTests.ts 파일에 reflect-metadata를 import해준다.

#### 4. @ 데코레이터를 사용하기 위해선 `tsconfig.json` 파일 속성 추가

```json
"experimentalDecorators": true,
"emitDecoratorMetadata": true
```

#### 5. container에 등록되어서 resolve를 통해 CounterStore 접근 (의존성 주입)

```jsx
// scr/components/Counter.tsx

import { container } from 'tsyringe'; 

import useForceUpdate from '../hooks/useForceUpdate';

import CounterStore from '../stores/CounterStore'; 

export default function Counter() {
  const store = container.resolve(CounterStore);
  const forceUpdate = useForceUpdate();

  const handleClick = () => {
    store.count += 1;
    forceUpdate();
  };

  return (
    <div>
      <p>{store.count}</p>
      <button type="button" onClick={handleClick}>
        Increase
      </button>
    </div>
  );
}
```

#### ✅ 실습을 통해 알게 된 점

- Counter/ CounterControl 컴포넌트로 분리 ⇒ CounterStore의 count 공유 방법
  - Counter → 화면에 출력되는 갯수
  - CounterControl → `Increase` 버튼
- Counter 컴포넌트가 2개 경우 ⇒ count 값이 동일하게 적용 될 수 있도록 처리하는 방법
  - new Set
  - useEffect (add, delete)
- 관심사 분리 ⇒ External Store(CounterStore)을 활용해 hooks으로 분리해서 재사용 하는 방법

<br/>

### singleton(싱글톤)

- 단 하나의 유일한 객체를 만들기 위한 코드 패턴
- __메모리 절약을__ 위해 인스턴스가 필요할 때 __똑같은 인스턴스를 새로 만들지 않고 기존의 인스턴스를 가져와 활용하는 기법__
- 대표적인 예시 → 데이터베이스 연결 모듈

> 우리가 전역 변수라는걸 만들어서 사용하는 이유는 똑같은 데이터를 메서드마다 지역 변수로 선언해서 사용하면 재사용성 측면에서 낭비, 전역에서 한번만 데이터를 선언하고 가져와 사용하면 효율적이기 때문이다.

싱글톤 기법은 오직 한개의 인스턴스 생성을 보증하여 효율을 찾을 수 있지만, 문제점들이 수반되기 때문에 균형잡힌 선택을 하는 것이 중요하다.
그래서 직접 유저가 만들어 사용하는 것보다는 프레임워크의 도움을 받아 사용해서 문제점들을 보안하면서 장점의 혜택을 누릴 수 있다.
(내부적으로 클래스의 제어를 IoC(Inversion of Control)방식의 컨테이너에게 넘겨 관리하기 때문에)

<br/>

## 🔗 참고

- [React 에서 의존성 주입 활용하기](https://velog.io/@woohm402/dependency-injection-in-reactjs)
- [TypeScript로 알아보는 의존성 주입](https://velog.io/@woohm402/dependency-injection-with-TypeScript)
- [리엑트 의존성 주입](https://velog.io/@jay/react-dependency-injection)
- [reflect-metadata](https://jeonghwan-kim.github.io/2023/06/20/reflect-metadata)
- [싱글톤 패턴](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
