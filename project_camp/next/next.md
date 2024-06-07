# Next.js 14

## 학습키워드

- 설치 방법
- Component
- App Router
  - 파라미터
  - 경로 탐색

<br/>

## Next.js

- React를 기반으로 웹 개발하는 프레임워크
  - 프레임워크는 도구가 주도권을 가진다. 프레임워크가 정해둔 규칙을 사용해서 개발한다.

<br/>

### 설치 방법

#### 설치

- 필요한 패키지를 직접 설치하여 프로젝트 세팅하는 방법

```shell
npm init -y

npm install react@latest next@latest react-dom@latest
```

#### Next.js 보일러 플레이트(boilerplate) 설치

```shell
npx create-next-app@latest
```

```shell
npx create-next-app@latest <프로젝트이름>
  ✔ Would you like to use TypeScript? … Yes  # 타입스크립트 사용 여부
  ✔ Would you like to use ESLint? … Yes  # ESLint 사용 여부
  ✔ Would you like to use Tailwind CSS? … Yes  # Tailwind CSS 사용 여부
  ✔ Would you like to use `src/` directory? … No  # src/ 디렉토리 사용 여부
  ✔ Would you like to use App Router? (recommended) … Yes  # App Router 사용 여부
  ✔ Would you like to customize the default import alias (@/*)? … No  # `@/*`외 경로 별칭 사용 여부
```

<br/>

## Component

- Next.js SSR 기반으로 동작한다. (Severe Side Rendering)
- 서버컴포넌트와 클라이언트 컴포넌트가 존재
- 기본적으로 서버 컴포넌트이다.
- 클라언트 컴포넌트로 변경 하고자 한다면 최상단에 `use client` 기재

> ✅ 클라이언트 컴포넌트 또한 일부 정적요소는 서버에서 렌더링한다. ‘server + client’ 의 하이브리드(hydration)컴포넌트로 이해해야한다. (hydration이란? 다시 화면을 그리는게 아닌, 자바스크립트의 기능 더하는것)

<br/>

## App Route

- Next.JS 14에서 새롭게 도입된 라우팅 시스템
- Routing 하기 위한 파일 규칙(File Conventions)이 존재한다.
  - 기본파일 `layout` 부터 순서대로 계층 구조를 나타내며, 각 페이지를 출력하기 위해 명시적 파일 규칙을 사용 해야 한다.

#### 📖 용어 정의

- **Routing** : 웹 애플리케이션에서 사용자가 URL을 통해 다른 페이지로 이동하는 것을 의미

- **Router** : Routing을 관리하고 처리하는 기능을 제공
- **Route** : URL과 특정 컴포넌트 간의 매핑

### 📄 Layout

- 여러 하위 경로에서 공통으로 사용하는 UI는 각 라우팅 폴더의 `layout.tsx` 컴포넌트 작성
  - 슬로(slot)방식으로 **Children Prop**를 사용하며, {children} 부분에는 같은 레벨에 있는 `page.tsx` 컴포넌트 출력

### 📄 Page

- 파일 시스템 기반의 라우터 방식을 사용
- **`/app` 폴더 내에 생성하는 각 폴더는 기본적으로 URL 경로를 의미**
  - **매핑되는 각 경로 구간을 세그먼트(Segment) 라고 한다.**
- **`/app/movies` 폴더를 생성하면 → <http://localhost:3000/movies> 의미**
  - **접근한 그 경로에서 출력할 내용은 기본적으로 각 폴더의 `page.tsx` 컴포넌트 작성**
- Routing 파일 규칙에 해당하는 이름이 아닌 파일은, 경로로 정의 되지 않는다.
  - 같은 폴더 안에서 자유롭게 파일을 추가해서 사용 할 수 있다. → (파일명) Routing Group 생성

![출처 : Next.js](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fdefining-routes.png&w=3840&q=75)

### 📄 not-found

- 페이지를 찾을 수 없을 때 보이는 not-found 페이지는 Next.js 프레임워크에서 ‘not-found.tsx’ 파일로 제어할 수 있다. 가장 최상위에 있는 경로에 생성

> 🤔 만약 다른경로에서 다른 레이아웃의 not-found 페이지를 생성하고 싶다면? <br/> 포괄적 경로 사용

```
app
├── not-found.tsx
└── category
    ├── [...slug]
    │   └── page.tsx → Not found layout
    ├── movie
    │   └── page.tsx
    └── genre
        └── page.tsx
```

#### 📂 중첩(Nested Route)

- 특정 폴더 안에 새로운 폴더를 중첩하는 형태로 중첩 경로 지정이 가능

```
app
└── blog
    ├── page.tsx → /blog
    └── detail/
        └── -page.tsx → /blog/detail
```

#### 📂 다이나믹 경로

- 동적(dyamaic)경로를 사용하면 특정 세그먼트가 동적으로 변경되는 경로를 지정

```
app
└── blog
    ├── page.tsx → /blog
    └── [id]/
        └── page.tsx → /blog/1 or /blog/2 or /blog/3...
```

#### 📂 중첩 다이나믹 경로

- 동적 경로도 중첩이 가능
- 동적 경로 이름을 다르게 지정해야 한다.

```
app
└── blog
    ├── page.tsx → /blog
    └── [id]
        ├── page.tsx → /blog/1 or /blog/2 or /blog/3...
        └── comment
            └── [reviewId]
                └── page.tsx.  → /blog/2/comment/1
```

#### 📂 포괄적 경로 (catch all segments)

- 특정 경로 이하의 모든 경로를 포괄적으로 허용하는 라우트 지정 방식

```
app
└── docs
    └── [...slug]
        └── page.tsx → /docs/1 or /docs/1/api or /docs/1/api/1 등등 모든 경로 커버 가능
```

#### 🗂️ Group

- 폴더를 생성하는 즉시 라우트 경로에 반영된다. 소괄호로 생성한 폴더 안에 넣어두게 되면 소괄호는 경로에 인식되지 않는다.

```
app
  (home)
    page.tsx → /
  (auth)
    login
      page.tsx → /login
    register
      page.tsx → /register
```

#### 🔐 프라이빗 폴더

- 폴더에 언더스코어를 붙이면 라우팅을 통해 접근할 수 없다.

```
app
  _utils
    page.tsx → 접근 불가 ❌
    form-date.ts
```

<br/>

## 파라미터

### Param props

- 서버 컴포넌트에서 path 정보를 props을 통해 확인 할 수 있다.

```jsx
// path : http://localhost:3000/blog/3?lang=ko&page=1

type TPageProps = {
  params: {
    [key: string]: string,
  },
  searchParams: {
    [key: string]: string,
  },
};

export default function page({ params, searchParams }: TPageProps) {
  return (
    <>
      <h1>page Component</h1>
      <p> 동적 경로 : {params.id}</p>
      <p> Query String : lang - {searchParams.lang}</p>
      <p> Query String : page - {searchParams.page}</p>
    </>
  );
}
```

### usePathname

- 클라이언트 컴포넌트에서 라우팅의 경로 반환하는 hook

```jsx
// path : http://localhost:3000/blog/3?lang=ko&page=1

'use client';

import { usePathname } from 'next/navigation';

export default function Page() {
  const pathname = usePathname();
  return (
    <>
      <h1>page Component</h1>
      <p>해당 페이지의 경로 : {pathname}</p> /// blog/3 (Query string 정보는 알수
      없음)
    </>
  );
}
```

### useSearchParams

- 클라이언트 컴포넌트에서 queryString 문자열을 가져올 때 사용하는 Hook
- `get`메소드를 사용해서 사용

```jsx
// path : http://localhost:3000/blog/3?lang=ko&page=1

'use client';

import { useSearchParams } from 'next/navigation';

export default function Page() {
  const queryString = useSearchParams();
  console.log(queryString.get('lang'));
  console.log(queryString.get('page'));
  return (
    <>
      <h1>page Component</h1>
      <p>해당 페이지의 경로 : {queryString.get('lang')} </p>
      <p>해당 페이지의 경로 : {queryString.get('page')} </p>
    </>
  );
```

<br/>

## 경로탐색

### Link 컴포넌트

- 페이지 이동을 위해 `<a>` 태그가 아닌 `<Link>` 컴포넌트 사용
- `prefetch` 옵션을 통해 뷰포트에 보여질 때(IntersectionObserver), 연결된 경로(href)의 데이터를 미리 가져와 탐색 성능을 크게 향상시킬 수 있다.
  - `null`(기본값) : 정적인 경로인 경우 모든 하위 경로, 동적 경로인 경우 `loading.tsx`가 있는 가장 가까운 세그먼트까지 미리 가져온다.
  - `true` : 정적 경로와 동적경오 모두 미리 가져온다.
  - `false` : 미리 가져오지 않는다.

> ⚠️ Prefetch 기능은 제품(Production)모드에서만 활성화 된다.

```jsx
import Link from 'next/link';

export default function Links() {
  return (
    <>
      <Link href={someLink}>이동~</Link>
      <Link prefetch={true} href={someLink}>
        이동~
      </Link>
      <Link prefetch={false} href={someLink}>
        이동~
      </Link>
    </>
  );
}
```

### useRouter

- 이벤트처리 내에서 페이지 이동시 사용하는 hook
- 클라이언트 컴포넌트에서만 사용할 수 있어 `use client` 선언 필요

```jsx
'use client';

import { useRouter } from 'next/navigation';

export default function Home() {
  const router = useRouter();
  console.log(router);
  return <>...</>;
}
```

#### back

- 현재페이지 기준으로 이전 히스토리(history)로 이동 (브라우저의 뒤로가기 동일)

```jsx
'use client';

import { useRouter } from 'next/navigation';

const useRouterHandler = () => {
  router.back(); // 이전 페이지 히스토리로 이동
};
```

#### forward

- 현재페이지 기준으로 다음 히스토리 이동(브라우저의 앞으로 가기 동일)

```jsx
'use client';

import { useRouter } from 'next/navigation';

const useRouterHandler = () => {
  router.forward();
};
```

#### prefetch

- 페이지를 미리로드하여 빠른 탐색을 가능하게 한다.

> ⚠️ Prefetch 기능은 제품(Production)모드에서만 활성화 된다.

```jsx
'use client';

import { useEffect } from 'react';
import { useRouter } from 'next/navigation';

export default function Header() {
  const router = useRouter();
  useEffect(() => {
    router.prefetch('/movies');
  }, [router]);
  return (
    <header>
      {/* 생략 */}
      <button onClick={() => router.push('/movies')}>MOVIES</button>
    </header>
  );
}
```

#### push

- 새로운 URL로 이동하며, 브라우저 기록에 새로운 항목을 추가

```jsx
'use client';

import { useEffect } from 'react';
import { useRouter } from 'next/navigation';

export default function Header() {
  const router = useRouter();
  useEffect(() => {
    router.prefetch('/movies');
  }, [router]);
  return (
    <header>
      {/* 생략 */}
      <button onClick={() => router.push('/movies')}>MOVIES</button>
    </header>
  );
}
```

#### replace

- 새로운 URL로 이동하지만, 브라우저 기록을 덮어쓴다.(기록에 남지 않는다.)

```jsx
const router = useRouter();

const useRouterHandler = () => {
  router.replace('/blog'); // /blog 페이지에서 뒤로가기 기능이 되지 않는다.(이전의 페이지 기록 없음)
};
```

#### refresh

- 현재 페이지를 다시 로드한다.

```jsx
const router = useRouter();

const useRouterHandler = () => {
  router.refresh();
};
```

### redirect

- redirect() 함수를 사용해서 라우팅 가능하다.

```jsx
import { redirect } from 'next/navigation';

export default function Product() {
  redirect('/');
  return (
    <>
      <h1>Product Component</h1>
    </>
  );
}
```
