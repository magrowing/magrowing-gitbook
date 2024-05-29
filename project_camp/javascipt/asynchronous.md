# 비동기적 프로그래밍

## 학습 키워드

- Callback 함수
- Promise
- async/await

<br/>

## Callback 함수

- 다른함수의 매개변수로 전달되는 함수를 의미
- 어떤 작업이 끝나거나 이벤트가 발생했을 때 호출되는 함수를 의미
- 일반적으로 비동기 함수 처리가 끝나면 실행할 작업으로 콜백함수를 전달

### 🤔 콜백함수가 사용되는 이유는?

자바스크립트는 `동기적 방식`으로 한번에 한가지 일만 처리한다. 즉, 함수가 호출 되는 순서대로 순차적으로 코드가 실행 된다.
그러나, 네트워크 요청과 같이 시간이 걸리는 작업을 수행 해야 하기 때문에 현재 실행 중인 코드가 종료 되지 않은 상태라해도 기다리지 않고 다음 코드를 바로 실행하도록 `비동기 방식`으로 코드를 실행시킨다. 이와 같이 순서가 보장 되지 않는 상황에서 순서가 보장되거나, 비동기 함수 처리가 끝나면 실행할 작업을 콜백함수에 전달하여 사용한다.

### 👩🏻‍💻 콜백함수 사용해보자

```x
function task1() {
  console.log('첫번째로 실행되어야 하는 함수');
}

function task2() {
  // ✅ 순서가 보장되지 않는 함수 로직이 있다면?
  setTimeout(() => {
    console.log('두번째로 실행되어야 하는 함수');
  }, 2000);
}

function task3() {
  console.log('세번째로 실행되어야 하는 함수');
}

function task4() {
  console.log('네번째로 실행되어야 하는 함수');
}

task1();
task2();
task3();
task4();
```

위의 코드를 보면 `task2` 함수내부에는 비동기로 동작하는 함수 `setTimeout` 존재한다. 내가 의도했던건 `task1 → task2 → task3 → task4` 순으로 동작하길 원했다. 그러나 `task1 → task3 → task4 → task2` 순으로 동작되어 의도한 방향과는 달랐다. 그렇다면 콜백함수를 사용해서 순서를 보장하는 코드를 작성해보자!

```javascript
function task1(callback) {
  console.log('첫번째로 실행되어야 하는 함수');
  callback();
}

function task2(callback) {
  // ✅ 순서가 보장되지 않는 함수 로직이 있다면?
  setTimeout(() => {
    console.log('두번째로 실행되어야 하는 함수');
    callback();
  }, 2000);
}

function task3(callback) {
  console.log('세번째로 실행되어야 하는 함수');
  callback();
}

function task4(callback) {
  console.log('네번째로 실행되어야 하는 함수');
  callback();
}

task1(() => {
  task2(() => {
    task3(() => {
      task4(() => {
        console.log('모든 함수 실행 완료!!');
      });
    });
  });
});
```

### 😱 콜백지옥(Callback hell)

![출처 : maxlchan.log](https://velog.velcdn.com/images/o1011/post/21a1f154-466f-4cd4-bfd1-e9659da01e10/image.png)

- 비동기 함수가 처리가 끝나면 실행되어야 할 콜백함수들이 중첩되어 indent(들여쓰기)가 점점 깊어지는 형태로 되고 이와같이 코드의 가독성이 안좋아지게 되는 상황을 `콜백지옥`, `Callback hell`이라고 부른다.

<br/>

## Promise

- 콜백함수의 단점을 보완하며 비동기 처리를 보다 효율적으로 처리하기 위해 ES6 추가된 문법
- 자바스크립트의 특수 객체 (`비동기 함수의 결과를 담고 있는 객체`)
- 두개의 콜백함수를 인자로 전달받는데, 작업이 성공적으로 완료되었을 때 결과를 반환하는 `resolve 함수`와 작업이 실패했을떄 에러를 반환하는 `reject 함수`가 있다.

### 기본 문법

- new 키워드를 통해 생성
- resolve, reject를 매개변수로 하는 함수 전달받는다.
  - `resolve` : 비동기 처리가 성공했을 때 호출할 함수
  - `reject` : 비동기 처리가 실패했을 때 호출할 함수

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('success');
    reject('fail');
  }, 2000);
});

promise
  .then((res) => {
    console.log(res); // success
  })
  .catch((err) => {
    console.log(err); // fail
  });
```

### 🔄 Promise 3가지 상태

#### pending(대기)

- `new Promise()` 메서드로 호출하면 대기 상태가 된다.
- 비동기 처리 로직이 아직 완료 되지 않은 상태

#### fulfilled(이행)

- 비동기 작업이 완료되어 프로미스가 결과값을 반환해준 상태
- `then` 을 통해 처리값을 전달받아 추가적인 처리 가능

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('success'); // 비동기 함수가 완료되면 resolve()함수 실행
  }, 2000);
});

promise.then((res) => {
  // resolve()의 결과값 콜백함수의 인자로 전달받아 사용 가능
  console.log(res);
});
```

#### rejected(실패)

- 비동기 작업이 실패하거나 오류가 발생한 상태
- 실패상태가 되면 실패한 이유를 `catch`로 받을 수 있다.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('fail'); // 비동기 함수가 실패하면 reject()함수 실행
  }, 2000);
});

promise.catch((res) => {
  // reject()의 실패이유를 전달 받아 추가적인 처리 가능
  console.log(res);
});
```

### ⛓️ Promise 체이닝

- Promise 객체의 메서드를 연결지어 사용하는 상황

```javascript
promise
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  });
```

### 😱 Promise hell도 존재한다

- 아래 코드와 같이 then 메서드를 통해 결과값을 인자로 받아와서 작업을 수행하고, 다시 then 메서드를 호출하고, 또 then,then,... 와 같이 작업이 무수히 길어지기 때문에 콜백지옥에 이어 `프로미스 헬`이라는 표현도 있다.

```javascript
function sum(num) {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      if (typeof num === 'number') {
        resolve(num + 1);
      } else {
        reject('num이 숫자가 아닙니다.');
      }
    }, 2000);
  });

  return promise;
}

sum(0).then((result) => {
  console.log(`결과 : ${result}`);
  sum(result).then((result2) => {
    console.log(`결과 : ${result2}`);
    sum(result2).then((result3) => {
      console.log(`결과 : ${result3}`);
      sum(result3).then((result4) => {
        console.log(`결과 : ${result4}`);
      });
    });
  });
});
```

### 🔥 return 활용해 Promise hell 해결하자

- return로 `promise`객체를 반환해서 사용하면 hell에서 벗어날 수 있다.

```javascript
sum(0)
  .then((result) => {
    console.log(`결과 : ${result}`);
    return sum(result); // 👈🏻 promise 반환하는 함수
  })
  .then((result2) => {
    console.log(`결과 : ${result2}`);
    return sum(result2);
  })
  .then((result3) => {
    console.log(`결과 : ${result3}`);
    return sum(result3);
  })
  .then((result4) => {
    console.log(`결과 : ${result4}`);
    return sum(result4);
  })
  .catch((err) => {
    console.log(`결과 : ${err}`);
  });
```

### Promise의 정적 메서드

#### [Promise.all()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

- 여러개의 비동기 처리를 `병렬적`으로 처리할 때 사용한다.
- 각가의 비동기 호출 순서가 상관 없을 때 사용한다.
- 주어진 프로미스 중 하나가 거부하는 경우, 전체를 reject 처리한다.

```javascript
function getEmoticon(icon) {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      if (typeof icon === 'string') {
        resolve(icon);
      } else {
        reject('이모티콘을 넣어주세요!');
      }
    }, 2000);
  });

  return promise;
}

Promise.all([getEmoticon('🍎'), getEmoticon('🍌'), getEmoticon('🍉')]).then(
  (results) => {
    console.log(results); // [ '🍎', '🍌', '🍉' ]
  }
);
```

#### [Promise.race](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

- 여러개의 비동기 처리 중 가장 먼저 `fulfilled` 된 처리 결과를 가져올 때 사용

```javascript
function getEmoticon(icon, ms) {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      if (typeof icon === 'string') {
        resolve(icon);
      } else {
        reject('이모티콘을 넣어주세요!');
      }
    }, ms);
  });

  return promise;
}

Promise.race([
  getEmoticon('🍎', 4000),
  getEmoticon('🍌', 2000),
  getEmoticon('🍉', 1000),
]).then((results) => {
  console.log(results); // 🍉 만 반환됨.
});
```

#### [Promise.allSettled](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)

- 여러개의 비동기 처리가 모두 `fulfilled` 나 `reject` 된 처리 결과를 resolve하는 promise를 반환

```javascript
function getEmoticon(icon, ms) {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      if (typeof icon === 'string') {
        resolve(icon);
      } else {
        reject('이모티콘을 넣어주세요!');
      }
    }, ms);
  });

  return promise;
}

Promise.allSettled([
  getEmoticon('🍎', 4000),
  getEmoticon(2, 2000),
  getEmoticon('🍉', 1000),
]).then((results) => {
  console.log(results);
});

// 출력결과
/**
[
  { status: 'fulfilled', value: '🍎' },
  { status: 'rejected', reason: '이모티콘을 넣어주세요!' },
  { status: 'fulfilled', value: '🍉' }
]
*/
```

<br/>

## async/await

- Promise를 조금 더 간편하고, 가독성 좋게 사용하기 위해 나온 문법

> ✅ async/await는 **promise** 를 기반으로 동작한다.<br> promise의 then,catch,finally 후속 처리 메서드에 콜백함수를 전달해서 비동기 처리 결과를 처리할 필요 없이 마치 동기처럼 프로미스를 사용할 수 있다.

### 기본문법

- 함수 앞에 `async`를 붙이면 `비동기적인 함수이고, Promise를 반환한다` 라고 선언하고, 반환 값이 Promise 생성함수가 아니어도 반환되는 값을 Promise 객체에 넣는다.

```javascript
const getData = async (url) => {
  const response = await fetch(url); // 👈🏻 Promise의 상태 변경이 되기전까진 다음코드는 실행되지 않는다.
  const result = await response.json();
  console.log(result);
};
```

- `await`은 `async`함수 안에서만 사용할 수 있고, Promise를 반환하는 함수 앞에 `await`를 붙이면 Promise가 성공 상태 또는 실패 상태로 바뀌기 전까지는 다음 연산을 하지않아 동기적으로 작동된다.

### async/await 예외처리

- Promise도 후속 처리가 많아질수록 then 내부에 코드가 중첩되면서 코드 흐름이 복잡해지는 상황을 보안하기 위해 `try/catch` 으로 간단히 처리 할 수 있다.

```javascript
const getData = async (url) => {
  try {
    const response = await fetch(url);
    const result = await response.json();
    console.log(result);
  } catch (error) {
    console.log(error);
  }
};
```

<br/>

## 🔗 참고

- [[JavaScript] 콜백함수, 프로미스, async/await](https://velog.io/@o1011/JavaScript-Promise-와-ansyc-await)
- [FE 인터뷰 준비 - 자바스크립트(2)](https://velog.io/@shyuuuuni/FE-%EC%9D%B8%ED%84%B0%EB%B7%B0-%EC%A4%80%EB%B9%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B81-nytbufb3#%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98%EB%A5%BC-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94)
- [예제로 이해하는 async/await 문법](https://velog.io/@tosspayments/예제로-이해하는-awaitasync-문법)
