# Next Auth

## 학습키워드

- 설치 및 세팅

<br/>

## Next Auth

### 설치 및 세팅

> Next14 버전은 현재 호환성 문제 있음 (24.06.13 기준)

- 🔗 [공식문서 참고](https://authjs.dev/)

```shell
npm install next-auth@beta
```

#### 터미널에 입력해서 나온 문자를 환경변수로 지정

```shell
openssl rand -base64 33
```

```
AUTH_SECRET= 환경변수지정
```

#### src/auth.ts

```typescript
import NextAuth from 'next-auth';

export const { handlers, signIn, signOut, auth } = NextAuth({
  providers: [],
});
```

#### app/api/auth/[...nextauth]/route.ts

```typescript
import { handlers } from '@/auth'; // Referring to the auth.ts we just created
export const { GET, POST } = handlers;
```

#### src/middleware.ts

- 🚨 배포를 위해 bulid 하면 해당 파일 error

```typescript
export { auth as middleware } from '@/auth';
```

- 해당 코드를 사용로 변경 해야함

```typescript
import { NextRequest, NextResponse } from 'next/server';
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|favicon.ico).*)'],
};
export async function middleware(request: NextRequest) {
  return NextResponse.next();
}
```
