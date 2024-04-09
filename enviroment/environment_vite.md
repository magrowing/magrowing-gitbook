# 개발 환경

## Vite

### React + TypeScript + ESlint

```shell
npm create vite@latest
```

- React
- TypeScript
- Eslint

### jest & React Testing Library

```shell
npm i -D jest @types/jest ts-jest(?)
npm i -D @testing-library/react jest-environment-jsdom @testing-library/jest-dom 
npm i -D jest-svg-transformer identity-obj-proxy 
```

#### `jest.config.ts` 설정 파일 생성

```ts
export default {
  testEnvironment: "jsdom",
  transform: {
    "^.+\\.tsx?$": "ts-jest",
  },
  moduleNameMapper: {
    "^.+\\.svg$": "jest-svg-transformer",
    "\\.(css|less|sass|scss)$": "identity-obj-proxy",
  },
  setupFilesAfterEnv: ["<rootDir>/src/setupTests.ts"],
};
```

> module 사용하면 이슈 있는데 아직 정확한 이유를 몰라 ts로 생성

```js  
module.exports = {
  testEnvironment: "jsdom",
  transform: {
    "^.+\\.tsx?$": "ts-jest",
  },
  moduleNameMapper: {
    "^.+\\.svg$": "jest-svg-transformer",
    "\\.(css|less|sass|scss)$": "identity-obj-proxy",
  },
  setupFilesAfterEnv: ["<rootDir>/src/setupTests.ts"],
};
```

#### `setupTests.ts` 설정 파일 생성

```ts  
{
  setupFilesAfterEnv: ["<rootDir>/src/setupTests.ts"],
};
```

> `jest.config.ts` 설정한 root로 설정해야한다.

```ts
// src/setupTests.ts

import '@testing-library/jest-dom'; // 👈🏻 작성해야함.
```

#### package.json `scripts` 추가

```json
"scripts": {
  // ...(중략)...
  "test": "jest",
  "watch:test": "jest --watchAll"
},
```

<br/>

### 🔗 참고

- [Vite 프로젝트 Jest 추가하기](https://velog.io/@thdrldud369/Vite-프로젝트에-jest-추가하기)
