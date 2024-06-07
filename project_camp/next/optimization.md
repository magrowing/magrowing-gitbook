# 최적화

## 학습키워드

- 메타데이터
- 폰트 최적화

<br/>

## 메타데이터

- 검색 엔진 최적화(SEO)를 위한 메타데이터를 각 페이지마다 정의 할 수 있다.

### 정적 데이터 생성

- 각 경로의 `layout.tsx` or `page.tsx` 파일에서 metadata 객체를 내보내면 된다.

```jsx
export const metadata = {
  title: '제목!',
  description: '설명..',
  openGraph: {
    title: '제목',
    // ...
  },
};

export default function RootLayout({
  children,
}: Readonly<{ children: React.ReactNode }>) {
  return (
    <html lang="ko">
      <body>
        <Header />
        {children}
      </body>
    </html>
  );
}
```

### 템플릿 제공

- `title`은 객체타입으로 지정해둔 템플릿(template)과 기본값(default)을 제공
- `%s` 치환문자에 동적으로 값이 삽입
  - 하위 경로에 정의된 제목에 사이트 이름 등을 접두사나 접미사로 추가할 때 유용

```jsx
export const metadata: Metadata = {
  title: {
    template: '%s | Next Movies',
    default: 'Next Movies',
  },
  description: 'He best movies on the best framework',
};
```

### [동적 데이터 생성](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)

- `generateMetadata` 함수를 사용
- 매개변수를 전달받아 처리 할 수 있어 API 요청으로 생성할 메타데이터를 가져올 수 있다.

```jsx
import { Metadata, ResolvingMetadata } from 'next'

type Props = {
  params: { id: string }
  searchParams: { [key: string]: string | string[] | undefined }
}

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  // read route params
  const id = params.id

  // fetch data
  const product = await fetch(`https://.../${id}`).then((res) => res.json())

  // optionally access and extend (rather than replace) parent metadata
  const previousImages = (await parent).openGraph?.images || []

  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  }
}

export default function Page({ params, searchParams }: Props) {}
```

<br/>

## 폰트

- 지원하는 모든 글꼴 파일에 대한 자체호스팅이 내장되어 있다. 기본적으로 모든 Google Fonts를 자체 호스팅으로 지원, 구글 API로 별도로 요청을 전송하지 않는다.

#### 내장 폰트 함수 초기화

```jsx
import { Roboto } from 'next/font/google';

export const roboto = Roboto({
  subsets: ['latin'], // 사용할 폰트 집합
  weight: ['400', '700'], // 사용할 폰트 두께
  display: 'swap', // 폰트 다운로드 전까지 기본 폰트 표시(성능 최적화)
  variable: '--font-roboto', // 사용할 CSS 변수 이름
});
```

#### className 속성으로 폰트 적용

```jsx
import { roboto } from '@/styles/fonts';

export default function Headline() {
  return (
    <>
      <h1 className={roboto.className}>OMDb API</h1>
    </>
  );
}
```

#### CSS 변수를 사용해서 폰트 적용

```jsx
import { roboto } from '@/styles/fonts';
import '@/styles/global.scss';

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode,
}>) {
  return (
    <html lang="ko" className={`${roboto.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

<br/>

## 🔗 참고

- [Next.js 핵심 정리](https://www.heropy.dev/p/n7JHmI)
