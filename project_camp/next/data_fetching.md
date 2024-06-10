# Data Fetching

## 학습키워드

- Suspense
  - Streaming with Suspense
- Client vs Server Component

<br/>

## Suspense

- 실제 콘텐츠가 로딩되는 동안 보여주는 `대체 콘텐츠` 의미
- React 18 추가된 컴포넌트

> Suspense를 사용하면 스켈레톤 및 스피너와 같은 로딩표시기를 pre-rendering 할 수 있다. 로딩시간이 길어질동안 사용자가 흰 화면먼 보고 있는것이 아닌, Loading의 의미의 UI를 보여주는것 → 더나은 UX 경험제공

```jsx
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

### SSR이 페이지를 보여주기까지의 과정

![Server Side Rendering](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming-chart.png&w=3840&q=75)

#### 1. 해당 페이지에 대한 모든 데이터가 서버에서 가져온다

#### 2. 서버는 페이지의 HTML을 렌더링한다

#### 3. 페이지의 HTML, CSS 및 JavaScript가 클라이언트로 전송된다

#### 4. 생성된 HTML 및 CSS를 사용하여 비대화형(non-interactive) 사용자 인터페이스 <br/>(non-interactive : 사용자가 상호작용할 수 없는 정적인 상태)가 표시된다

#### 5. 마지막으로, React가 사용자 인터페이스를 hydrates하여 상호작용 가능하게 만든다

**위 단계에서 suspense는 4번의 단계에서 사용하게 된다.**
클라이언트(브라우저)가 서버에서 받은 HTML, CSS로 웹 페이지의 초기 버전을 로딩시켰지만, 아직 사용자는 상호 작용할 수 없는 단계이다. 상호작용을 하려면, 5단계에서 JavaScript와 React와 같은 라이브러리나 프레임워크가 Hydrate하는 과정이 끝나야 상호작용이 가능해진다.

> 즉, 4단계부터 5단계가 끝나는 과정까지, 사용자가 상호작용 할 수 없는 정적페이지를 마주해야하는 상황이 발생되는데, 이러한 시간에 suspense를 사용하여 로딩중이라는 UI보여줌으로서 더나은 UX 제공한다.

### Streaming with Suspense

![Server Rendering Rendering All UI](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming.png&w=3840&q=75)

페이지가 사용자에게 표시되기 전에 서버에서 모든 데이터 가져오기 완료되어야 하므로 **여전히 전체전인 UI 보여주는 속도가 느릴 수 있다.** 그러나 스트리밍을 사용하면 페이지의 HTML을 더 작은 청크로 나누고 점진적으로 해당 청크를 서버에서 클라이언트로 보낼 수 있다.(먼저 온 데이터는 먼저 보낸다.)

![Streaming with Suspense](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming.png&w=3840&q=75)

```jsx
import { Suspense } from 'react';
import { PostFeed, Weather } from './Components';

export default function Posts() {
  return (
    <section>
      <Suspense fallback={<p>Loading feed...</p>}>
        <PostFeed />
      </Suspense>
      <Suspense fallback={<p>Loading weather...</p>}>
        <Weather />
      </Suspense>
    </section>
  );
}
```

> ✅ Suspense로 비동기 작업(데이터패칭)을 수행하는 컴포넌트를 래핑하고 해당 작업이 진행되는 동안 대체 UI를 표시한 다음 작업이 완료되면 컴포넌트를 교체하는 방식으로 작동

#### Suspense를 사용하면 다음과 같은 이점을 얻을 수 있다

1. 스트리밍 서버 렌더링 - 서버에서 클라이언트로 HTML을 점진적으로 렌더링 한다.
2. 선택적 수화 - React는 사용자 상호작용을 기반으로 어떤 컴포넌트를 먼저 대화식으로 만들것인지 우선순위를 정한다.

<br/>

## 🔗 참고

- [공식문서 : Loading UI and Streaming](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)
- [React, Next.js | Suspense](https://velog.io/@ohjoo1130/React-Next.js-Suspense)
- [[Next 13] Routing - Loading UI와 Streaming](https://velog.io/@hyeon9782/Next-13-Routing-Loading-UI와-Streaming)
- [[Next 13] Routing - Loading UI and Streaming](https://rocketengine.tistory.com/entry/NextJS-13-Routing-Loading-UI-and-Streaming)
