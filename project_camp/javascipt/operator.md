# 연산자

## 학습 키워드

- 연산자
  - 산술 연산자
  - 증감 연산자
  - 대입 연산자
  - 비교 연산자
  - 삼항 연산자
  - 논리 연산자
  - null 병합 연산자

<br/>

## 연산자

- 어떤 특정한 연산을 처리하기 위해서 사용하는 기호

### 피연산자

- 연산자에 의해서 연산에 참여하는 데이터(값) 또는 항을 의미

### 산술 연산자

- 사칙 연산을 다룰 때 사용하는 연산자
  - `+` 덧셈
  - `-` 뺄셈
  - `*` 곱셈
  - `/` 나눗셈
  - `%` 나머지
- 산술연산자의 우선순위 : `*`, `/`, `%`
- `()` 통해 우선 순위 적용

### 증감 연산자

- 증가 연산자와 감소 연산자를 일컫는 용어

#### 전위 연산자 (++변수, --변수)

- 수를 증가시키고 증가/감소 후의 값을 반환

```javascript
let num = 10;

console.log(++num); // 11
console.log(--num); // 10
```

#### 후위 연산자 (변수++, 변수--)

- 수를 증가시키고 증가/감소 하기 전의 값을 반환

```javascript
let num = 20;

console.log(num++); // 20
console.log(num--); // 21
```

> 후위 연산자의 대표적인 예시 for문

```javascript
for (let i = 1; i < 10; i++) {
  console.log(i); // 1,2,3,4,5,6,7,8,9
}
```

### 대입 연산자

#### [할당 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Assignment)

- 변수에 값을 대입하는데 사용

#### 복합 대입 연산자

- `+=` ,`-=`, `*=`, `/=`, `%=`

```javascript
let num = 10;
num = num + 10; // 👈🏻 num +=20 이런식으로 사용 가능
```

### 비교 연산자

#### 동등 비교 연산자

- `===` : 자료형(type)까지 비교하는 연산자
- `==` : 자료형(type)까진 확인 하지 않는 연산자

```javascript
let comp1 = 1 === '1';
let comp2 = 1 == '1';
console.log(comp1, comp2); // false, true
```

#### 대소 비교 연산자 : `>`, `<`, `>=`, `<=`

### 삼항 연산자

- 항 3개를 사용하는 연산자
- 조건식 `?` 참일 경우 반환값 `:` 거짓을 경우 반환값

```javascript
let var4 = 10;
let res = var4 % 2 === 0 ? '짝수' : '홀수';
console.log(res); // 짝수
```

### 논리 연산자

- Boolean 타입의 True / False 값을 다룰 때 사용하는 연산자

```javascript
let or = true || false;

let and = true && false;

let not = !true;

console.log(or); // true
console.log(and); // false
console.log(not); // false
```

### null 병합 연산자

- `??`
- null, undefined 아닌 값을 찾아내는 연산자

```javascript
let var1;
let var2 = 10;
let var3 = 20;

console.log(var1 ?? var2); // 10
console.log(var1 ?? var3); // 20
console.log(var2 ?? var3); // 10  👈🏻 null, undefined 없는 경우 앞에 있는 값을 출력함
```
