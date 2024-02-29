# E2E Test

## 학습 키워드

- E2E Test
- E2E Test Tool
  - Puppeteer
    - Headless Chrome
    - Headless Browser
  - Playwright
  - CodeceptJS

<br/>

## E2E Test

### 📖 E2E Test

- End To End 테스트의 약자
- __사용자 중심으로__ 처음부터 끝까지 어플리케이션 흐름을 테스트하는 소프트웨어 테스트 방법
- 실제 사용자 시나리오를 시뮬레이션하고 어플리케이션 구성 요소의 통합 및 데이터 무결성을 검증하는 것

__⇒ 실제 사용자의 관점으로서 애플리케이션 동작 흐름을 테스트__

<br/>

## E2E Test Tool

### 🛠️ [Puppeteer](https://pptr.dev/)

- Headless Chrome 혹은 Chromium 를 제어하도록 도와주는 라이브러리

> Node 6 이상 에서 동작하며, Chrome 혹은 Chromium 의 DevTools 프로토콜 을 통해 각 Chrome 혹은 Chromium 의 API 제어한다.

#### 🤖 Puppeteer를 통해 할 수 있는 작업

- 페이지의 스크린샷 및 PDF를 생성한다.
- SPA(Single-Page Application)를 크롤링하여 미리 렌더링된 콘텐츠(즉, "SSR"(Server-Side Rendering).양식 제출, UI 테스트, 키보드 입력 등을 자동화 한다.
- 최신 자동화된 테스트 환경을 만들수 있다. 최신 자바스크립트 및 브라우저 기능을 사용하여 크롬의 최신 버전에서 직접 테스트를 실행한다.
- 사이트의 타임라인 추적을 캡처하여 성능 문제를 진단할 수 있다.
- 크롬 확장 플러그인을 테스트할 수 있다.

#### 📖 [Headless Chrome](https://developer.chrome.com/blog/headless-chrome?hl=ko)

- 헤드리스 환경에서 Chrome 브라우저를 실행하는 방법
- Chrome 없이 Chrome을 실행하는 것이라고 할 수 있다.

#### 📖 Headless Browser

- 그래픽 사용자 인터페이스(GUI)가 없는 웹 브라우저
- CLI(Command Line Interface)에서 동작하는걸 뜻한다.

<br/>

### 🛠️ [Playwright](https://playwright.dev/docs/intro/)

- 웹 브라우저 기반 E2E 테스트 자동화 도구
- 하나의 API로 모든 최신 브라우저(크로미움, 파이어폭스, 웹킷)에서 빠르고, 안정적인 자동화를 지원하는 MS에서 만든 자동화 도구 (Edge나 IE11은 지원하지 않음)

#### ⚙️ 설치 및 사용 방법

- 브라우저 및 서버 실행

```
npm start 
npx nodemon app.ts
```

- Playwright 패키지 설치

```shell
npm npm i -D @playwright/test eslint-plugin-playwright
```

- `playwright.config.ts` 파일 생성

```ts
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
 testDir: './tests',
 retries: 0,
 use: {
  channel: 'chrome',
  baseURL: 'http://localhost:8080', //테스트 할 URL
  headless: !!process.env.CI, 
  screenshot: 'only-on-failure', // 실패시 
 },
};

export default config;
```

> ✅ headless: !!process.env.CI <br/>
 CI 환경이 잡혀있을 경우 헤드리스로 띄우기(아닌 경우 그냥 띄움)

- 📁 tests/`.eslintrc.js` 파일 생성

```shell
mkdir tests

touch tests/.eslintrc.js
```

```js
module.exports = {
    env: {
        jest: false, // js 사용하지 않을거라서
    },
    extends: ['plugin:playwright/playwright-test'],
    rules: {
        'import/no-extraneous-dependencies': 'off',
    },
};
```

- 📁 tests/`home.spec.ts` 파일 생성

```ts
import { test, expect } from '@playwright/test';

test('Filter products', async ({ page }) => {
 await page.goto('/');

 await expect(page.getByText('Apple')).toBeVisible();

 const searchInput = page.getByLabel('Search');

 await searchInput.fill('a');

 await expect(page.getByText('Apple')).toBeVisible();

 await searchInput.fill('aa');

 await expect(page.getByText('Apple')).toBeHidden();
});

test('Click the “Increase” button', async ({ page }) => {
 await page.goto('/');

 const count = 13;

 await Promise.all((
  [...Array(count)].map(async () => {
   await page.getByText('Increase').click();
  })
 ));

 await expect(page.getByText(`${count}`)).toBeVisible();
});
```

- `.gitignore`파일에 에러 상황의 스크린샷 등이 담기는 `test-results` 디렉터리 추가.

```
/test-results/
```

#### 🤖 테스트 실행

```shell
npx playwright test

# headless로 실행(브라우저를 띄우지 않아 훨씬 빠르다.)
CI=true npx playwright test 
```

<br/>

### 🛠️ [CodeceptJS](https://codecept.io/)

- 인간 친화적인 E2E 테스팅 도구
- Playwright 기본으로 사용 할 수 있다.

#### ⚙️ 설치 및 실행 방법

- [CodeceptJS 3 시작하기](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)
- [CodeceptJS 사용](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-%EC%82%AC%EC%9A%A9)

<br/>

## 🔗 참고

- [카카오 기술 블로그 E2E 테스트 도입 경험기](https://fe-developers.kakaoent.com/2023/230209-e2e/)
- [E2E 테스트 구축기 (used AWS Step Functions)](https://medium.com/delivus/e2e-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EA%B5%AC%EC%B6%95%EA%B8%B0-used-aws-step-functions-2fccb930218c)
- [프론트엔드 테스트 - TDD와 종류(Unit, Integration, E2E)](https://soojae.tistory.com/74)
- [puppeteer 개념과 예제](https://velog.io/@nias0327/puppeteer-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%98%88%EC%A0%9C)
- [Puppeteer 간단 정리하기](https://pks2974.medium.com/puppeteer-간단-정리하기-a252bffbb2a8)
