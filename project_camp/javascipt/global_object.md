# 표준 내장 객체

## 학습 키워드

- 표준 내장 객체
  - 배열 내장 객체
  - 문자 내장 객체
  - Math 내장 객체

<br/>

## [표준 내장 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects)

- 자바스크립트 엔진에 기본으로 `내장되어 있는 객체`를 의미

### [배열 내장 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)

#### [push](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

- 배열의 값을 맨 뒤에서부터 추가
- 원본 배열 수정 O
- 배열의 새로운 길이 반환

#### [shift](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

- 배열의 첫 번째 요소를 제거
- 원본 배열 수정 O
- 제거된 요소를 반환

#### [unshift](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

- 배열의 값을 맨 앞에서부터 추가
- 원본 배열 수정 O
- 배열의 새로운 길이 반환

#### [join](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

- 지정된 구분문자열로 연결한 새문자열을 만들어 반환

```javascript
const elements = ['Fire', 'Air', 'Water'];
console.log(elements.join('-')); // "Fire-Air-Water"
```

#### ⭐️ [sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

- 배열 요소들을 정렬할 경우 사용
- 원본 배열 수정 O
- 문자열의 유니코드를 기준으로 정렬
- 숫자의 경우 인자로 비교함수를 사용한 방식으로 정렬 가능

```javascript
const number = [1, 2, 100, 20, 40];

// 오름차순 정렬
number.sort((a, b) => {
  return a - b;
});

// 내림차순 정렬
number.sort((a, b) => {
  return b - a;
});
```

```javascript
// 객체들로 구성된 배열 정렬
const user = [
  { name: '김똥개', age: 24 },
  { name: '홍길동', age: 20 },
  { name: '아무개', age: 25 },
];

user.sort((a, b) => a.age - b.age);
// [
//   { name: '홍길동', age: 20 },
//   { name: '김똥개', age: 24 },
//   { name: '아무개', age: 25 }
// ]
```

#### [reverse](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

- 배열의 순서를 반전
- 원본 배열 수정 O

```javascript
const number = [1, 2, 3, 4, 5];
number.reverse(); // [5,4,3,2,1]
```

#### [forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

- 각 배열의 요소에 대한 제공된 함수를 한번씩 실행
- 순회 메서드
- 항상 `undefined` 반환

#### [find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

- 제공된 배열에서 제공된 함수를 만족하는 `첫번째 요소` 반환
- 테스트 함수를 만족하는 값이 없으면 `undefined` 반환

```javascript
const array = [5, 12, 8, 130, 44];
const found = array.find((element) => element > 10);
console.log(found); // 12
```

#### ⭐️ [filter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

- 주어진 배열의 일부에 대한 얕은 복사본을 생성 → `새로운 배열` 반환
- 주어진 배열에서 제공된 함수에 의해 구현된 테스트를 통과한 요소로만 필터링

```javascript
const array = [5, 12, 8, 130, 44];
const found = array.filter((element) => element > 10);
console.log(found); // [12,130,44]
```

#### ⭐️ [map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

- 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 `새로운 배열` 반환

```javascript
const array = [1, 2, 3, 4, 5];
const found = array1.filter((element) => element * 2);
console.log(found); // [2,4,6,8,10]
```

#### ⭐️ [reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

- 각요소에 대한 주어진 함수를 실행하고, `하나의 결과값`을 반환

```javascript
// array.reduce(callback[, initialValue]);

배열.reduce(() => {}, 초기값);
```

```javascript
[0, 1, 2, 3, 4].reduce((prev, curr) => prev + curr, 0);
```

<br/>

### [문자 내장 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)

#### ⭐️ [split](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)

- 지정한 구분자를 기준으로 끊은 부분 문자열을 다음 배열 반환

```javascript
const fruits = 'apple,banana,orange';

console.log(fruits.split(',')); // ["apple", "banana", "orange"]
```

#### ⭐️ [includes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/includes)

- 지정한 문자열이 포함되어 있는지를 판별하고, `true/false` 반환

```javascript
const fruits = 'apple,banana,orange';

console.log(fruits.includes('apple')); //true
console.log(fruits.includes('color')); //false
```

#### [charAt](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)

- 문자열에서 특정 인덱스에 위치하는 유니코드 `단일문자`를 반환

```javascript
const fruits = 'apple';

console.log(fruits.charAt(0)); // a
console.log(fruits.charAt(3)); // l
```

#### [concat](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/concat)

- 매개변수로 전달된 모든 문자열을 호출 문자열에 붙인 `새로운 문자열` 반환

```javascript
const str1 = 'Hello';
const str2 = 'World';

console.log(str1.concat(' ', str2)); // "Hello World"
console.log(str2.concat(', ', str1)); // "World, Hello"
```

#### ⭐️ [slice](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

- 문자열의 일부를 추출하여 `새로운 문자열` 반환
- `endIndex` : 그 직전까지 추출, 해당 인덱스 위치 문자는 추출에 포함 X

```javascript
str.slice(beginIndex[, endIndex])
```

```javascript
const str = 'hello/word';
console.log(str.slice(0, 5)); // 'hello'
```

<br/>

### [Math 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math)

- 수학적인 상수와 함수를 위한 속성과 메서드를 가진 내장 객체
- `Math` 생성자가 아니며, 모든 속성과 메서드는 `정적`이다.

#### [min](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/min)

- 주어진 숫자들 중 가장 작은 값을 반환

#### [max](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

- 주어진 숫자 중 가장 큰 수를 반환
- 매개변수가 없을 경우 : `-Infinity` 반환

#### [random](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

- 0보다 크거나 1보다 작은 부동 소수점 의사 난수를 반환

#### 🧐 만약 최소와 최대 범위를 지정하고 싶다면?

```javascript
Math.random() * (max - min) + min;
```

```javascript
let result = Math.random() * (45 - 1) + 1;
console.log(result); // 1.000000000 ~ 44.9999999999
```

#### [round](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

- 소수점 반올림

```javascript
let result = Math.round(1.1);
console.log(result); // 1
```

#### [floor](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

- 소수점 내림

```javascript
let result = Math.floor(1.1);
console.log(result); // 1
```

#### [ceil](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/ceil)

- 소수점 올림

```javascript
let result = Math.ceil(1.1);
console.log(result); // 2
```

#### [abs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/abs)

- 주어진 숫자의 절대값을 반환

```javascript
let result = Math.abs(5 - 3);
console.log(result); // 2

let result = Math.abs(3 - 5);
console.log(result); // 2
```

#### [pow](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/abs)

- x의 y 거듭제곱을 반환

```javascript
let result = Math.pow(2, 3); // 2의 3승 = 8
console.log(result); // 8
```
