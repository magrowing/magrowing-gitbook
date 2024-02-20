# TypeScript

## 학습 키워드

- REPL
- TypeScript
  - `타입 정의하기(Defining Types)`
  - 타입 추론
    - `Tuple`
  - Interface vs Type
  - Union Type vs Intersection Type
  - Optional Parameter
  - `Generics` ~~추후작성~~
  - `Utility Types` ~~추후작성~~

<br/>

## REPL

### 📖 REPL은 무엇인가?

- Read-Eval-Print-Loop의 약자
- 애플리케이션에서 사용자가 입력한 명령어(소스코드)를 읽고(Read) 명령어를 평가하고(Eval) 결과를 출력(Print)한 다음 다시 입력한 상태를 기다리는 상태로 돌아가는 과정을 반복(Loop)한다.

<br/>

### 💡 REPL은 왜 사용하는가?

- 주로 개발 진행시 소스코드를 빠르게 실행하여 결과를 즉각적으로 확인하기 위한 용도 사용 __⇒ 테스트 용도__
- 프로그래밍언어를 입문한 경우에 유용하게 사용

> [ex]  C#을 배우기 위해 컴퓨터에 VisualStudio를 설치하거나 Java를 배우기 위해 Eclipse 또는 Intelij를 설치하는 과정은 입문자에게는 어려울 수 있으며, 설치 후 환경설정, 프로젝트를 관리하는 것도 굉장히 번거롭다. 이때 REPL을 사용해 언어를 익힐 수 있다.

<br/>

### 🤖 REPL은 어떻게 사용하는가? (with ts-node)

#### 📖 ts-node는 무엇인가?

- TypeScript 실행기이면서 Node.js용 __REPL__
  
#### 💡 ts-node은 왜 사용하는가?

- 타입스크립트는 __컴파일 언어__ 이기 때문에 자바스크립트로 변환이 필요하다.
- ts-node는 타입스크립트를 자바스크립트로 변환해주기 때문에 컴파일 없이도 Node.js에서 타입스크립트를 직접 실행할 수 있음
  __⇒ 개발 환경에서 타입스크립트 코드를 테스트 하기 위해 사용한다.__

#### 🤖 ts-node은 어떻게 사용하는가?  

   ```shell
   # 설치 없이 실행 
   npx ts-node

   # 종료 
   Ctrl + C or .exit
   ```

<br/>

### ✍🏻 REPL / ts-node 에 대한 나의 생각

- TypeScript 기초적인 문법 공부시 TypeScript playground를 통해 학습했는데 vscode에서 위와 같은 방식을 사용해서 공식문서의 예제들을 테스트 하면서 되겠다.

<br/>

### 🔗 참고

- [REPL 참고 블로그]("https://developer-talk.tistory.com/542")
- [ts-node 참고 Gitbook]("https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week1/typescript")

<br/>

## TypeScript

### 📖 [TypeScript]("https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html") 무엇인가?

- __JavaScript에 타입을 부여한 언어__ __⇒JavaScript의 확장된 언어__
- Microsoft에 의해서 개발 및 관리되고 있는 오픈소스 프로그래밍 언어
- 대규모 애플리케이션을 개발하는데 JavaScript가 어렵고 불편하다는 불만에 대응하기 위해서 개발되었다.
 __⇒ Javascript의 단점 보안 해주는 언어__

<br/>

### 💡 TypeScript 왜 사용하는가? (장점 vs 단점 )

- 정적 타입 언어 `에러 사전 방지`, `안정성`, `협업용이성`
- 실행 속도

> JavaScript은 RunTime 이후 브라우저의 개발자 도구인 `console`을 통해 에러를 확인 할 수 있다. 그러나  TypeScript는 브라우저에서 실행 할 수 없다. TypeScript는 컴파일 언어이기 때문에 JavaScript로 변환하여 사용한다. 변환하는 과정 === 즉, 컴파일 하는 단계에서 에러를 확인 할 수 있다. 에러가 노출되면 Javascript 변환 되지 않아 `에러를 사전 방지 할 수 있기에 안정적이다.`

```tsx
  function sum(a:number, b:number) : number{
    return a + b;
  }

  sum('10','20') // ERROE 
```

  > TypeScript 타입을 부여하는 언어이기에 매개변수와 반환값에 타입을 지정한다.명시적인 정적 타입 지정은 개발자의 의도를 명확하게 코드로 기술 할 수 있게 한다. 코드의 가독성을 높이기 때문에 `협업하는 과정에 유용하다.`

  > JavaScript는 RunTime 시 타입을 결정해서 적용 된다. 이때 오류가 있는지 확인하는 작업이 추가 되어 실행 속도가 오래 걸린다. 하지만, TypeScripts는 코드 작성 할 때 `타입을 미리 결정하기 때문에` 오류를 확인 하는 과정을 줄여 `실행 속도가 빠르다.`

***

- 초반 환경 세팅이 불편하다. `컴파일 하기 위한 환경 세팅`
- 타입을 기재 하지 않으면 TypeError로 인한 빨간 줄
- interface or class 등의 이름 때문에 오류 직면
- 가독성이 상대적으로 떨어진다. `코드 길어지는 점`
- JavaScript 에서 생기는 오류를 다 TypeScript 해결 할 순 없다. `언어 이자 도구`

<br/>

### 🤖 TypeScript 어떻게 사용하는가? (feat.기본 개념)

#### 1. 타입 정의하기(Defining Types)

- ` : ` 사용해서 type을 정의한다.

```tsx
/* 원시타입 */ 
let name: string = '홍길동';
let age: number = 13;
let bool : boolean = true; 

/* 배열 */ 
let numberArr:number[] = [1,2,3]; 
let numberArr:Array<string> = ['a','b','c'];

/* 객체 */ 
const human: {name: string; age: number;} = { name: '홍길동', age: 13 };

/* 함수 */
function(a:number,b:number) : number{
  return a + b; // 함수의 return 값은 number 타입이다.
}
```

<br/>

#### 2. 타입 추론

- 명시적으로 선언하지 않아도 TypeScript는 타입을 추론한다.

```tsx
// 명시 하지 않아도 humanLangue 변수는 string Type이라고 인지한다.
let humanLangue = '인간의 언어 한글이군요!' 

// 정해진 값으로 지정 Union Type 유용하게 사용 가능 
let category: 'food';
category = 'food';
```

##### 2-1. Tuple

- 배열을 더 상세하게 타입으로 관리하고 싶다면 사용

```tsx
let pair: [string, number];
pair = ['hp', 256];
```

#### 3. Interface vs Type

- Interface와 Type은 매우 유사하다. 둘 중 자유롭게 선택해서 사용하면 된다.
- 둘 다 `복잡한 오브젝트의 타입을 재사용`하기 위해 사용하는 것이다.
- `핵심적인 차이`Type은 새 프로퍼티를 추가하도록 개방될 수 없는 반면, Interface의 경우 항상 확장될 수 있다는 점
- [타입 별칭과 인터페이스의 차이점]("https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD%EA%B3%BC-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90")

```tsx
/* interface 아래 같은 방식으로 프로퍼티 확장 가능 */
interface Person {
  name:string; 
  age:number;
  gender:'male'|'female';
}

interface Person{
  year:number; //extends 방식으로 확장하는 방법도 있다.
}

const Kim :Person = {name:'김토끼', age:14, gender:'female',year:2002}
const Hong :Person = {name:'홍길동', age:18, gender:'male', year:2003}
```

```tsx
/* type 아래 같은 방식으로 프로퍼티 확장 불가능 */

type Human = {
  name:string,
  age:number,
}

type Human = {
  gender:'male'|'female';
}
// Error: Duplicate identifier 'Human'.

/*type 아래 같은 방식으로 프로퍼티 확장*/

type Human = {
  name:string,
  age:number,
}

type Gender = Human & {
  gender:'male'|'female';
}

const Lee:Gender = {name:'이용', age:22, gender:'male'}
```

#### 3-1. Type

- 객체를 생성하는 방식처럼 타입을 선언해준다.

```tsx
type Human = {
  name: string,
  age :number, 
  gender : 'male' | 'female';
}

const Hong:Human = {name:'홍길동',age:15, gender:male}
```

#### 3-2. Interface

- class 생성하는 방식처럼 타입을 선언해준다.

```tsx
  interface Person {
    name: string;
    age: number;
    gender: 'male' | 'female';
  }

  const Kim:Person = {name:'김토끼',age:19, gender:female}
```

<br/>

#### 4. Union Type

- `|`
- 여러 타입 중 하나 (`합집합`)
- 매개변수를 제한 할 때 매우 유용
- 레거시 환경 또는 코드에서 사용(?) ~~이해하지못함~~

```tsx
type Category = 'food' | 'bag' | 'toy';

function fetchProducts( {category} : { category: Category }) {
  console.log(`Fetch ${category}`);
}

fetchProducts({category:'food'});
```

#### 5. Intersection Type

- `&`
- `교집합`
- 타입을 확장하기 위해 사용  

```tsx
type Color = { color:string}
type Background = {backgroundColor:string}

const css : Color & Background = {color:'red', backgroundColor:'white'}; 
```

```tsx
interface Box {
  width: number; 
  height : number; 
}

interface Shape {
  shape : string; 
}

const boxModel : Box & Shape = {width:2000, height:2000, shape:'square'}
```

#### 6. Optional Parameter

- `?` 사용해서 파라미터를 선택적으로 사용 가능하게 한다.
- 매개변수가 오브젝트일 때 많이 활용

```tsx
function greeting(name?: string): string {
  return `Hello, ${name || 'world'}`;
}

greeting();
// 'Hello, world' 파라미터에 아무값도 넣어주지 않으면 name undefined 이기 때문에 
// false || 'world' 출력 

greeting('kim'); // 'Hello, kim'
```

<br/>

### ✍🏻 TypeScirpt에 대한 나의 생각

- 기본 개념을 이해했지만 실제 프로젝트를 진행해서 한다면 내가 과연 잘 사용 할 수 있을까 싶다.
- TypeScript를 왜 사용하는지는 알겠다. 그러나 왜 사용해야 하는지는 와닿지 않는다. 사용하다보면 필요성을 느끼게 되겠지?
- 해당 개념들은 아직 숙지 하지 못함  `Generics` / `Utility Types` 추가 공부가 필요해보인다.

<br/>

### 🔗 참고

- [TypeScript란 참고 블로그]("https://hymndev.tistory.com/79")
- [TypeScript 장단점 참고 블로그]("https://imraccoon-developer.tistory.com/11")
