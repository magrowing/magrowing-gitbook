# 1. 개발 환경

## 학습 키워드

* Node.js
  * `fnm(Fast Node Manager)`
* NPM(Node Package Manager)
  * package.json / package-lock.json
  * node\_modules
  * npx
* ES Modules vs CommonJS

***



## 개발 환경 세팅

`Node.js는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다.`  
런타임은 특정 언어로 만든 프로그램들을 실행할 수 있는 환경을 뜻한다.

* JavaScript를 브라우저 외부에서 실행할 수 있게 해주는 런타임 환경이다.

개발 환경을 Node.js로 세팅을 한다. toolchain이라고 부르는 개발 환경, 도구들의 모음을 Node.js로 쓴다.  
Deno를 사용하면 훨씬 간단하지만, 대부분의 서비스들은 Node.js로 구축되어 있기 때문에 하지만 Node.js를 통한 개발 환경 세팅은 생각보다 까다롭다.


어려운 이유는 도구가 계속 변화하기 때문이다. 새로운 도구가 나오면 그곳으로 쏠리고 계속 변화한다.  
강의에서는 전체적인 흐름을 파악하며 새로운 도구가 나오면 이렇게 써봐야지, 이런건 이러한 문제를 해결하려고 나왔구나 등의 접근을 해보는 것이 좋다.  
새로운 기술을 명목적으로 쫒을 필요는 없지만, 항상 뒤쳐지지 않게 트렌드에는 민감하자. 실질적으로 좋은 것이 있다면 도입하자. trade-off를 고려하자.


지금의 세팅도 나중에는 조금씩 달라질 것이다.  
바뀐 부분이 있으면 문서를 찾아서 업데이트 하면 되고 새로운 도구를 선택할 수도 있다.







## JavaScript 개발 환경 (Node.js) 세팅

Node.js를 설치하고, 프로젝트를 진행할 수 있는 Node.js 패키지를 만들고,  
코드 퀄리티를 일정 수준 이상으로 유지할 수 있도록 lint와 test를 실행할 수 있는 상태를 만든다.

* [Node.js](https://nodejs.org/en)
  * 터미널을 통해 버전 확인 할 수 있는 명령어 `node -v`
* [fnm (Fast Node Manager)](https://github.com/Schniz/fnm)
  * 계속 업그레이드되는 Node.js로 프로젝트를 진행하다 보면 프로젝트마다 서로 다른 버전을 사용하는 경우
  * 여러 버전의 Node.js를 설치해서 사용하고 싶을 때가 있는데, [fnm](https://github.com/Schniz/fnm)을 사용하면 이게 가능하다.
