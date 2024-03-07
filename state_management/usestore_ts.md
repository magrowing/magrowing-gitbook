# usesotre-ts

## 학습키워드

- usesotre-ts

<br/>

## [usesotre-ts](github.com/seed2whale/usestore-ts)

- 시드웨일에서 제작한 React state management library

> 간단한, 하지만 TypeScript 전용인, React 상태 관리 라이브러리 usestore-ts 입니다. React 외부(정확히는 UI 레이어가 아닌 코어!)를 내가 원하는 방식(FP or OOP)으로 구성하고 Flux처럼 단방향 흐름으로 엮어내는 니즈에 맞춰진 도구입니다.

<br/>

### 🤖 usesotre-ts 설치 및 설정

#### usesotre-ts 설치

```shell
npm install usestore-ts
```

#### Typescript @ 데코레이터를 사용하기 위해선 `tsconfig.json` 파일 속성 추가

```json
"experimentalDecorators": true,
"emitDecoratorMetadata": true,
```

<br/>

### 👩🏻‍💻 간단한 예제를 통해 실습

> 기존에 TSyringe + reflect-metadata를 사용하여 만든 간단한 스토어를 usestore-ts 를 사용해보기

#### Store 생성

usestore-ts에서 제공하는 Store 데코레이터는 기존 ObjectStore 클래스를 대체한다.
즉, ObjectStore가 제공하는 기능 (구독 관리, 상태변경을 알리는 작업 등)을 담고 있다는 것

```jsx
// CounterStore.ts

import { singleton } from 'tsyringe';

import { Store, Action } from 'usestore-ts';

/* 
 * 싱글톤 데코레이터를 스토어 데코레이터 보다 상위에 두자.
 * 싱글톤을 먹은 클래스를 스토어 데코레이터를 사용하면 이슈가 있을 수 있음
 */ 
@singleton()
@Store()
export default class CounterStore {
  count = 0;
  // 액션 데코레이터는 publish 메서드를 머금고 있다.
  @Action()
  increase(step = 1) {
    this.count += step;
  }

  @Action()
  decrease(step = 1) {
    this.count -= step;
  }
}
```

#### Custom Hook 작성

usestore-ts가 제공하는 useStore 훅으로 기존 useObjectStore를 대체
forceUpdate 리스너를 머금고 있다.

```jsx
// useCounterStore.ts

import {container} from 'tsyringe';

import {useStore} from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
    const store = container.resolve(CounterStore);

    return useStore(store);
}
```

#### const [ state , store ] = useStore(counterStore) 반환값 사용하기

```jsx
import useCounterStore from '../hooks/useCounterStore';

export default function Counter() {
  const [{ count }] = useCounterStore();  // 상태값만 사용

  return (
    <div>
      <p>{`count: ${count}`}</p>
    </div>
  );
}
```

```jsx
// CounterControl.tsx

import useCounterStore from '../hooks/useCounterStore';

export default function CounterController() {
  const [, store] = useCounterStore(); // 스토어의 Action만 사용

  const handleClickIncrease = (step?: number) => () => {
    store.increase(step);
  };

  const handleClickDecrease = (step?: number) => () => {
    store.decrease(step);
  };

  return (
    <div>
      <button type="button" onClick={handleClickIncrease(10)}>
        Increase 10
      </button>
      <button type="button" onClick={handleClickIncrease()}>
        Increase
      </button>
      <button type="button" onClick={handleClickDecrease(10)}>
        Decrease 10
      </button>
      <button type="button" onClick={handleClickDecrease()}>
        Decrease
      </button>
    </div>
  );
}
```

<br/>

### ⚠️ 사용시 주의 사항

비동기 함수에 @Action을 붙이면 다르게 작동할 수 있다는 점에 주의!
별도의 액션을 만들면 신경 쓸 부분이 줄어든다.

```ts
@singleton()
@Store()
class PostStore {
  posts: Post[] = [];
  
  //@Action() 👈🏻 이런식으로 사용하면 다르게 동작 할 수 있다?  
  async fetchPosts() {
    this.startLoading();
  
    const posts = await fetchPosts();

    this.completeLoading(posts);
  }
  
  @Action()
  startLoading() {
    this.posts = [];
  }
  
  @Action()
  completeLoading(posts: Post[]) {
    this.posts = posts;
  }
}
```

<br/>

## 🔗 참고

- [다른 기수 메가테라 GitBook usestore-ts](https://taewoongs-organization.gitbook.io/dev-road/dev_road/week-6/usestore-ts)
- [다른 기수 메가테라 GitBook usestore-ts](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week6/usestore-ts)
