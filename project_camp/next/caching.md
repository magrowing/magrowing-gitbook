# Caching

## 학습키워드

- Router Cache
- Full Route Cache
- Request Memoization
- Data Cache

<br/>

## [Caching](https://nextjs.org/docs/app/building-your-application/caching#router-cache)

> 📖 캐시 : 컴퓨터 과학에서 데이터나 값을 미리 복사해 놓은 임시 장소

> 📖 캐싱 : 어떤 데이터를 한번에 받아온 후에 그 데이터를 불러온 저장소보다 가까운 곳에 임시로 저장하여, 필요시 더 빠르게 불러와서 사용하는 프로세스

Next.js는 성능을 향상시키고 비용을 절감하기 위해 최대한 캐시한다. **경로는 정적으로 렌더링 되고, 데이터 요청은 캐시 된다는것을 의미한다.**

![Caching](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fcaching-overview.png&w=3840&q=75)

<br/>

### Router Cache

Next.js에는 사용자 세션 동안 개별 경로 세그먼트로 분할된 `React Server Component Payload(RSC Payload)`를 저장하는 in-memory를 클라이언트 측, 캐시를 의미한다.

![How the Router Cache Works](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Frouter-cache.png&w=3840&q=75)

> `http://localhost:3000/a`라는 페이지 요청을 하면 `Router Cache`는 해당 페이지의 캐시기록을 확인하다. Miss로 판단하게 되면 캐시하기 위한 절차를 진행한다. `http://localhost:3000/b`라는 페이지를 요청하면 `Router Cache`는 해당 페이지의 캐시기록을 확인하고 Miss로 판단하게 되면 캐시하기 위한 절차를 진행한다. 다시 `http://localhost:3000/a` 라는 페이지를 요청하면 `Router Cache`는 해당 페이지의 캐시기록을 판단하고, 기록이 있다면(HIT) 페이지를 다시 로드 하지 않고, React와 브라우저 상태를 캐싱한 컨텐츠를 사용자에게 보여준다.

사용자가 라우트간을 이동하는 동안 Next.js가 방문한 라우트 세그먼트를 캐시하고, 사용자가 이동할 가능성이 있는 경로를 미리 가져옴으로서 사용자의 네비게이션 경험을 향상시킨다.

<br/>

### Router Cache 지속시간

캐시는 브라우저의 임시 메모리에 저장된다. 라우터 캐시의 지속시간을 결정하는 두가지 요소는 다음과 같다.

- `Session` : 캐시는 탐색 전반에 걸쳐 지속된다. 그러나 **새로고침** 하면 지워진다.
- `자동 무효화 기간` : 개별 세그먼트의 캐시는 **특정시간이 지나면 자동으로 무효화 된다.** 이기간은 경로가 정적으로 동적으로 렌더링 되는지에 따라 달라진다.
  - 동적으로 렌더링 된 경우 : 30초
  - 정적으로 렌더링 된 경우 : 5분

> ⚠️ 개별 세그먼트가 마지막으로 엑세스 되거나 생성된 시간부터 영향을 받는다.

<br/>

#### 👩🏻‍💻 예제를 통해 라우터 캐시 작동 방식을 이해보자

```
app
├── layout.tsx
├── page.tsx
├── about → 정적경로
│   └── page.tsx
└── blog
    └── [id] → 동적경로
        └── page.tsx
```

```jsx
// 📂 app/layout.tsx
export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode,
}>) {
  return (
    <html lang="en">
      <body className="p-10">
        <h1 className="text-3xl font-bold mb-4">RootLayout Component</h1>
        <p className="mt-4 text-lg font-bold mb-10">
          {new Date().toLocaleTimeString()}
        </p>
        <nav className="flex gap-10 mb-10">
          <Link className="text-lg text-blue-700 " href={'/'}>
            /
          </Link>
          <Link className="text-lg text-blue-700" href={'/about'}>
            /About
          </Link>
          <Link className="text-lg text-green-700" href={'/blog/1'}>
            /blog/1
          </Link>
          <Link className="text-lg text-green-700" href={'/blog/2'}>
            /blog/2
          </Link>
        </nav>
        {children}
      </body>
    </html>
  );
}
```

> RootLayout.tsx 파일에는 `<Link>`컴포넌트를 사용해서 경로이동이 가능하도록 지정해두었다.

```jsx
// 개별세그먼트
// 📂 /about
// 📂 /blog/[id]
export default function page() {
  return (
    <section className="bg-gray-200 p-10">
      <h1 className="text-2xl font-bold">About page Component</h1>
      <p className="mt-4 text-lg font-bold">
        {new Date().toLocaleTimeString()}
      </p>
    </section>
  );
}
```

> /about, /blog/:id 정적경로와 동적경로를 지정한 페이지를 생성해두었고, 해당 페이지가 로드되면 현재 시간을 출력하도록 구현했다.

![적용된 화면](../images/router_cache.gif)

> 🧪 Router Cache가 적용되어, RootLayout의 컴포넌트에 적용된 시간은 캐싱되어 다른 경로 이동시 현재 시간을 보여주는게 아니라, 첫 RootLayout의 컴포넌트 렌더링시 출력되었던 시간을 유지해서 보여준다.

<br/>

### Full Route Cache

- Next.js에서 `라우트의 렌더링 결과를 캐시`하여 서버에서 클라이언트로의 반복적인 렌더링 요청을 줄이고, 페이지 로드 성능을 향상시킨다.

![Full Route Cache](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Ffull-route-cache.png&w=3840&q=75)

#### ✅ Full Route Cache vs Route Cache 차이점

- 라우터캐시 : 사용자 세션 동안 브라우저에 React 서버 구성요소 페이로드를 임시로 저장한다. 정적 및 동적으로 렌더링된 경로 모두 적용된다.

- 전체 경로 캐시 : 여러 사용자 요청에 걸쳐 서버에서 React 서버 구성요소 페이로드와 HTML을 지속적으로 저장한다. `빌드 또는 재검증 중에 정적으로 렌더링된 경로만 캐시한다.`

> ⭐️ 전체라우트캐시(Full Route Cache)는 서버측에서 페이지의 HTML렌더링 결과를 캐시하고, 라우터 캐시(Router Cache)는 클라이언트측에서 사용자가 방문한 경로 세그먼트를 캐시하여 사용자의 경로 이동을 더 빠르게 만든다.

<br/>

## 🔗 참고

- [프론트엔드 개발자가 알아야할 '캐싱' 개념정리](https://yozm.wishket.com/magazine/detail/2341/)
- [Next.js cache](https://velog.io/@j_wisdom_h/Next.js-cache)
- [Next.js 캐싱으로 웹 서버 성능 최적화](https://fe-developers.kakaoent.com/2024/240418-optimizing-nextjs-cache/)
