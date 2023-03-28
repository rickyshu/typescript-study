</br>

# 📌들어가기에 앞서

</br>

> 해당 포스트는 타입스크립트를 학습한 내용을 주관적인 기준에 따라 정리한 내용입니다.

</br>

---

</br>

# 📖 타입

</br>

## 2.1 타입의 종류

`타입`이란 `Javascript`에서 다루는 값의 <strong style="color:skyblue"> 형태</strong>에 대한 설명이다.

</br>

#### 🔗 형태란?

값에 존재하는 속성과 메서드 그래고 내장되어 있는 `typeOf` 연산자가 설명하는 것을 의미

- 일곱 가지 기본 원시 타입

  1. `null`

  2. `undefined`
  3. `boolean`
  4. `string`
  5. `number`
  6. `BingInt`
  7. `symbol`

</br>

### 2.1.1 타입 시스템

타입 시스템(type system)은 프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙 집합.

- 기본적인 타입스크립트의 타입 시스템 동작

  1. 코드를 읽고 존재하는 모든 타입을 이해
  2. 각 값이 초기 선언에서 가질 수 있는 타입을 확인
  3. 각 값이 추후 코드에서 어떻게 사용될 수 있는지 모든 방법 확인
  4. 값의 사용법이 타입과 일치하지 않으면 사용자에게 오류 표시

</br>

### 2.1.2 오류 종류

자주 접하게 되는 두 가지 오류

- <strong>구문 오류</strong> :

  - 타입스크립트가 자바스크립트로 변환되는 것을 차단한 경우

- <strong>타입 오류</strong> :
  - 타입 검사기에 따라 일치하지 않는 것이 감지된 경우

</br>

#### 구문 오류

- `Typescript`가 코드로 이해할 수 없는 잘못된 구문을 감지할때 발생

- 고문 오류가 발생하는 경우 컴파일을 차단
  (`javascript` 파일을 얻지 못하거나 얻더라도 예상과 다를 수 있음)

</br>

```ts
let let someting;
// Error : ',' expected
```

> 🔑 타입스크립트는 구문 오류와는 상관없이 자바스크립트 코드를 출력하기 위해서 최선을 다함. 하지만 원하는 출력 결과가 아닐 수 있음. 따라서 컴파일 된 `Javascript`를 실행하기 전에 구문 오류를 수정한는 것이 좋음

</br>

#### 타입 오류

`Typescript`의 `inference`로 오류를 감지했을 때 발생
오류가 발생했다고 컴파일을 차단하지 않음

> 🔑 `Javascript` 코드가 원하는 대로 실행되지 않을 가능성이 있다는 신호를 타입 오류로 알려준다. `Javascript`를 실행하기 전에 타입 오류를 확인하고 발견된 문제를 먼저 해결하는 것이 가장 좋다.

</br>

---

</br>

## 2.2 할당 가능성

`Typescript`는 변수의 초깃값을 읽고 해당 변수가 허용되는 타입을 결정

`Typescript`에서 함수 호출이나 변수에 값을 제공할 수 있는지 확인하는 것을 <strong style="color:skyblue"> 할당 가능성(assignability)</strong>라고 한다.

즉, 전달된 값이 예상된 타입으로 할당 가능한지 확인

</br>

## 2.3 annotation

초기 타입을 유추할 수 없는 변수는 <strong style="color:tomato"> 진화하는 any</strong> 라고 부른다.

```ts
let rocker; // type : any
rocker = 'Joan Jett'; // type : string

rocker.toUpperCase(); //OK

rocker = 19.58; // type : number
rocker.toPrecision(1); //OK

rocker.toUpperCase();
//Property 'toUpperCase' does not exist on type 'number'.
```

</br>

`typescript`는 초깃값을 할당하지 않고 변수의 타입을 선언할 수 있는 구문인 `type annotation` 이 존재한다.

`type annotation`은 변수 이름 뒤에 배치되며 콜론(:)과 타입이름을 차례대로 기재한다.

```ts
let rocker: string;
rocker = 'Joan Jett';
```

</br>

이러한 `Type annotation`은 `Typescript`에만 존재하며 런타임 코드에 영향을 주지도 않고, 컴파일 시 제거된다.

> 🔑 초기 할당으로 `TypeChecker`에 의해서 `inference`가 가능할 경우 `anntation`까지 중복해서 사용하지 않는 것이 좋다.

     (경우에 따라 다름)

</br>

## 2.4 타입 형태

타입의 형태가 정해졌다면 `Typescript`는 객체에서 어떤 멤버 속성이 존재하는지 알고 있기 때문에 해당 타입에서 사용가능한 속성을 자동완성할 수 있고, 잘 못된 부분이 있는지도 확인 가능하다.

```ts
// Type Annotation으로 구체적으로 명시가능
// 하지만 Inference가 가능한 경우 오히려 효율과 가독성이 떨어질 수 있음
const user: { firstName: string; age: number } = {
  firstName: 'hoseung',
  age: 33,
};
//Property 'middleName' does not exist on type '{ firstName: string; age: number; }'.
user.middleName;
```

`Typescript`는 객체의 형태에 대한 이해를 바탕으로 할당 가능성뿐만 아니라 객체 사용과 관련된 문제도 알려줍니다.

</br>

## 2.5 Module

한 모듈에서 다른 파일에 선언된 변수와 동일한 이름으로 선언된 변수는 다른 파일의 변수를 가져오지 않는 이상 이름 충돌이 발생하지 않는다.
(`export`로 내보내지 않으면 다른 파일에서 사용할 수 없다)

```ts
// a.ts
export const shared = 'koby';

// b.ts
export const shared = 'koby';

// index.ts
import { shared } from './a';
// Error: Import declaration confilcts with local declaration of 'shared'

export const shared = 'koby';
//Error : Individual declarations in merged declaration
//'shared' must be all exported or all local.
```

그러나 파일이 `script`일 경우 `Typescript`는 해당 파일을 `global scope`로 간주하므로 모든 `script`가 파일의 내용에 접근할 수 있다.

# 📚 레퍼런스

> Goldberg, et al. 러닝 타입스크립트 / 조시 골드버그 지음 ; 고승원 옮김, 2023.

> <a href="https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AA%A8%EB%93%88-%EB%84%A4%EC%9E%84%EC%8A%A4%ED%8E%98%EC%9D%B4%EC%8A%A4-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0" target="_blank"> 모듈 & 네임스페이스 시스템 이해하기 | Inpa Dev </a>

> <a href="https://www.typescriptlang.org/ko/docs/handbook/modules.html" target="_blank"> Modules | Typescript 공식문서 </a>

</br>

---

- 유니언(union): 값에 허용된 타입을 두 개 이상의 가능한 타입으로 확장하는 것

- 내로잉(narrowing): 값에 허용된 타입이 하나 이상의 가능한 타입이 되지 않도록 좁히는 것

</br>

## 📖 3.1 유니언(Union)

</br>

값이 정확히 어떤 타입 인지 모르지만 두 개 이상의 옵션 중하나라는 것을 알고 있는 경우 사용할 수 있다.

타입스크립트는 가능한 값 또는 구성 요소 사이에 `|` 연산자를 사용해 유니언 타입임을 나타낸다.

```js
// // something: string | number
let something: string | number;

// something: string | number
let something2 = Math.random() > 0.5 ? 'leekoby' : 1;

// "string"과 "number" 모두 갖고 있는 메서드
something2.toString();

// ❌ Error : Property 'toFixed' does not exist on type 'string'.
something2.toFixed(); // "number"만 갖고 있는 메서드

// ❌ Error : Property 'toUpperCase' does not exist on type 'number'.
something2.toUpperCase(); //"string" 만 갖고 있는 메서드
```

</br>

---

</br>

## 📖 3.2 내로잉(narrowing)

</br>

<strong style="color:skyblue">내로잉(narrowing)</strong>

- ㄱ값의 타입(union)의 범위를 좁히는 과정
- 값이 이전에 유추된 것보다 더 구체적인 타입임을 유추하는 것

<strong style="color:skyblue">타입가드(type guard)</strong>

- 타입을 좁히는 데 사용할 수 있는 논리적 검사

</br>

### 3.2.1 값 할당을 통한 내로잉

타입스크립트는 변수의 타입을 할당된 값의 타입으로 좁힌다.

```js
let admiral: number | string;
admiral = 'leekoby';

admiral.toUpperCase(); //OK : string

// ❌ Error : Property 'toFixed' does not exist on type 'string'. Did you mean 'fixed'?
admiral.toFixed();
```

```js
let admiral: number | string = 'leekoby';

admiral.toUpperCase(); //OK : string

// ❌ Error : Property 'toFixed' does not exist on type 'string'. Did you mean 'fixed'?
admiral.toFixed();
```

</br>

### 3.2.2 조건 검사를 통한 내로잉

조건문으로 유니언 타입을 좁힐 수 있다.

```ts
let something = Math.random() > 0.5 ? 'leekoby' : 1;

// type이 number 일 때로 타입 좁히기
if (something === 2) {
  // something: 2
  something.toFixed();
}
// type이 string 일 때로 타입 좁히기
if (something === 'leekoby') {
  // something: "leekoby"
  something.toUpperCase();
}

// ❌ Error : Property 'toUpperCase' does not exist on type 'number'.
something.toUpperCase();
```

</br>

### 3.2.3 typeof 검사를 통한 내로잉

`typeof` 연산자를 사용하여 타입을 좁힐 수 있음.

`typeof` 검사는 자주 사용하는 실용적인 방법

```js
let something = Math.random() > 0.5 ? 'leekoby' : 1;

// type이 number 일 때로 타입 좁히기
if (typeof something === 'number') {
  something.toFixed();
}
// type이 string 일 때로 타입 좁히기
if (typeof something === 'string') {
  something.toUpperCase();
}

// ! 를 사용한 논리 부정과 else 문도 가능
// type이 number 일 때로 타입 좁히기
if (!(typeof something === 'number')) {
  something.toUpperCase();
} else something.toFixed();
// type이 string 일 때로 타입 좁히기

// 삼항연산자도 사용가능
typeof something === 'string' ? something.toUpperCase() : something.toFixed();

// ❌ Error : Property 'toUpperCase' does not exist on type 'number'.
something.toUpperCase();
```

</br>

---

</br>

## 📖 3.3 리터럴 타입(literal type)

좀 더 구체적인 버전의 원시 타입

원시 타입 값 중에 하나가 아닌 <strong style="color:tomato">특정 원싯값</strong>으로 알려진 타입을 의미한다.

- `string` 이 아닌 "leekoby"라는 구체적인 타입을 리터럴 타입이라고 한다.

```ts
// const something:"leekoby"
const something = 'leekoby';
```

</br>

### 3.3.1 리터럴 할당 가능성

동일한 원시 타입이라고 해도 서로 다른 리터럴 타입이면 할당할 수 없다.

```ts
let something: 'leekoby' = 'leekoby';
something = 'Byron'; // ❌ Error: Type '"Byron"' is not assignable to type '"leekoby"'.
```

</br>

✅ 리터럴 타입은 원시 값에 할당 가능
❌ 원시 값은 리터럴 타입에 할당 불가

```ts
let sayHi: 'Hi';
sayHi = 'Bye'; //❌ 오류발생

sayHi = 'Hi';
let something = '';
something = sayHi; // ✅가능
```

</br>

## 3.4 엄격한 null 검사(strict null checking)

타입이 필요한 위치에서 null 값을 허용하는 많은 타입 시스템
const something: string = null;

타입스크립트는, `strictNullChecks` 옵션을 통하여 엄격한 `null` 검사 설정을 할 수 있다.
일반적으로 엄격한 `null` 체크를 활성화해야만 `null` 또는 `undefined` 값으로 인한 오류로 부터 안전한지 쉽게 파악가능

```ts
let something: string;

// strictNullChecks: true
something = null; // ❌ Error: Type 'null' is not assignable to type 'string'.

// strictNullChecks: false
something = null; // 문제 없음
```

> 🔑 엄격한 `null`검사를 활성화하는 것을 권장

</br>

### 3.4.2 참 검사를 통한 내로잉

`Javascript`에서는 `falsy`(`0, -0, 0n, "", false, null, undefined, NaN`) 같은 `falsy` 값을 제외하고 모두 참이다. 이를 이용하여 타입 추론을 할 수 있다.

```ts
let sth = Math.random() > 0.5 ? 'leekoby' : undefined;

if (sth) {
  sth.toUpperCase(); //✅
}
sth.toUpperCase();
// ❌ Error : sth' is possibly 'undefined'.
```

> 💢 논리 연산자의 부족한 점 :

- 참 여부 확인 외에 다른 기능은 제공하지 않음.
- `falsy` 값이라면 `""` 같은 빈 문자열인지 `undefined`인지 알수 없음

```ts
let sth = Math.random() > 0.5 && 'hello';
if (sth) {
  // sth: string
  sth;
} else {
  // sth : string | false
  sth;
}
```

</br>

### 3.4.3 초깃값이 없는 변수

</br>

`javascript`에서는 초깃값이 없는 변수는 기본적으로 `undefined`가 된다.

`Typescript` 에서는 값이 할당될 때까지 변수가 `undefined`임을 이해하고, 변수의 속성을 사용하려고 시도하면 오류를 발생시킨다.

```ts
let sth: string;
sth.length;
// ❌ Error : Variable 'sth' is used before being assigned.

let someth: string | undefined;

someth?.length; //✅

someth = 'leekoby';
someth.length; // ✅
```

</br>

## 3.5 타입 별칭

<strong style="color:skyblue">타입 별칭(type alias)</strong> :

- 재사용하는 타입에 더 쉬운 이름을 할당하는 것
- `type` 새로운이름 = `type` 형태이며 `pascalCase`로 작성

```ts
type sth = string | number;
let something: sth = 'leekoby';

type sth2 = boolean | number | string | null | undefined;
let first: sth2;
let second: sth2;
let third: sth2;
```

</br>

### 3.5.1 타입 별칭은 `Javascript`엔 없다

타입 별칭은 `annotion`처럼 자바스크립트로 컴파일되지 않는다. (런타임에 존재하지 않음)

위의 코드를 컴파일한 결과

```ts
'use strict';
let something = 'leekoby';
let first;
let second;
let third;
```

또한 타입 별칭은 타입 시스템에만 존재하므로 런타임 코드에서 참조할 수 없다.

```ts
console.log(sth, sth2);
// ❌ Error : 'sth' only refers to a type,
// but is being used as a value here.

// ❌ Error : 'sth2' only refers to a type,
// but is being used as a value here.
```

</br>

### 3.5.2 타입 별칭 결합

타입 별칭은 다른 타입 별칭을 참조할 수 있다.

```ts
type Id = number | string;
type Idmaybe = Id | undefined | null;
// IdMaybe의 타입은 number | string | undefined | null
```

```ts
type Idmaybe = Id | undefined | null;
type Id = number | string;
```

---

</br>

</br>

# 📚 레퍼런스

> Goldberg, et al. 러닝 타입스크립트 / 조시 골드버그 지음 ; 고승원 옮김, 2023.

> <a href="https://typescript-kr.github.io/pages/unions-and-intersections.html" target="_blank"> Typescript 핸드북 </a>

</br>

---

</br>

# 📖 4.1 객체 타입

- `{...}` 구문을 사용해서 객체 리터럴을 생성하면, TS는 해당 속성을 기반으로 새로운 객체 타입 또는 타입 형태를 고려한다.

- 해당 객체 타입은 객체의 값과 동일한 속성명과 원시 타입을 갖는다.
- 값의 속성에 접근하려면 value.멤버또는value["멤버"]를 구문을 사용한다.

```ts
const someone = {
  born: 1990,
  name: 'leekoby',
};
someone['born']; // 타입 : number
someone.name; // 타입 : string

someone.hello;
// ❌ Error : Property 'hello' does not exist on type '{ born: number; name: string; }'.
```

- 객체 타입은 `Typescript`가 `Javascript`를 이해하는 방법에 대한 핵심 개념

- `null`과 `undefined`를 제외한 모든 값은 그 값에 대한 실제 타입의 멤버 집합을 가짐.
- 그렇기 때문에 `Typescript`는 모든 값의 타입을 확인하기 위해서 객체 타입을 이해해야한다.

</br>

## 4.1.1 객체 타입 선언

- 기존 객체에서 직접 타입을 유추하는 방법도 굉장히 좋지만, 객체의 타입을 명시적으로 선언하고 싶음

- 명시적으로 타입이 선언된 객체와는 별도로 객체의 형태를 설명하는 방법 필요
- 객체 타입은 객체 리터럴과 유사하게 보이지만 필드 값 대신 타입을 사용해 설명

```
let someone = {
    born: 1990,
    name: "leekoby"
};

someone = {
    born: 1333,
    name: "kobykoby"
};
someone["born"] // 타입 : number
someone.name // 타입 : string

someone = "cube"
// ❌ Error : Type 'string' is not assignable to type '{ born: number; name: string; }'.
```

</br>

## 4.1.2 별칭 객체 타입

- 객체 타입을 계속 장성하는 일은 효율성이 떨어진다.

- 객체의 타입에 타입 별칭을 할당해 사용하는 방법이 더 일반적이다.
- ✅ 대부분의 타입스크립트 프로젝트는 객체 타입을 설명할 때 interface 키워드 사용을 선언한다.

```ts
type koby = {
  born: number;
  name: string;
};

let someone: koby;
someone = {
  born: 1990,
  name: 'leekoby',
};

someone = {
  born: 1333,
  name: 'kobykoby',
};
someone['born']; // 타입 : number
someone.name; // 타입 : string

someone = 'cube';
// ❌ Error : Type 'string' is not assignable to type 'koby'.
```

</br>

> 대부분의 `Typescript` 프로젝트는 객체 타입을 설명할 때 `interface`

# 🦆 4.2 구조적 타이핑

- 타입스크립트의 타입 시스템은 구조적으로 타입화 되어 있다.
  - 즉, 타입을 충족하는 모든 값을 해당 타입의 값으로 사용할 수 있다.
  - 매개변수나 변수가 특정 객체 타입으로 선언되면 타입스크립트에 어떤 객체를 사용하든 해당 속성이 있어야 한다

```ts
type WithFirstName = {
  firstName: string;
};

type WithLastName = {
  lastName: string;
};

const hasBoth = {
  firstName: 'Koby',
  lastName: 'Lee',
};

// ✅ hasBoth는 string 타입의 "firstName"을 포함함
let WithFirstName: WithFirstName = hasBoth;

// ✅ hasBoth는 string 타입의 "lastName"을 포함함
let WithLastName: WithLastName = hasBoth;
```

- <strong style="color:skyblue">덕 타이핑 ( `JavaScript` )</strong>:

  - 런타임에서 사용될 때까지 객체 타입을 검사하지 않는 것

- <strong style="color:skyblue">구조적 타이핑 ( `TypeScript` )</strong>:
  - 정적 시스템이 타입을 검사하는 것

</br>

## 4.2.1 사용검사

객체 타입에 정의한 값을 필수적으로 선언해야 하고, 타입의 정보와도 일치해야한다.

```ts
type Sth = {
  name: string;
  age: number;
};

//  Error: Property 'age' is missing in type '{ name: string; }' but required in type 'Sth'
const sth: Sth = {
  name: 'Aatrox',
};

// Error: Type 'string' is not assignable to type 'number'
const sthBoth: Sth = {
  name: 'Aatrox',
  age: '10',
};
```

</br>

## 4.2.2 초과 속성 검사

초기에 정한 타입보다 더 많은 속성을 사용하려하면 타입 오류가 발생

객체 리터럴이 아닌 경우에는 구조적 타이핑에 의해서 오류가 발생하지 않음

```ts
type Sth = {
  name: string;
  age: number;
};

// (3)
const Sth1: Sth = {
  name: 'leekoby',
  age: 33,
  activity: 'working',
  // ❌ Error : Type '{ name: string; age: number; activity: string; }' is not assignable to type 'Sth'.
  // Object literal may only specify known properties, and 'activity' does not exist in type 'Sth'.
};

// (4)
const Sth = {
  name: 'kobykoby',
  age: 22,
  activity: 'working',
};
// (4)
const Sth2: Sth = Sth;

// ❌ Error : Property 'activity' does not exist on type 'Sth'.
Sth2.activity;
```

</br>

## 4.2.3 중첩된 객체 타입

`Javascript` 객체는 다른 객체의 멤버로 중첩될 수 있으므로 타입스크립트의 객체 타입도 타입 시스템에서 중첩된 객체 타입을 나타낼 수 있어야한다

- 중첩 객체의 속성의 형태를 자체 별칭 객체 타입으로 추출하면 오류 메시지에 더 많은 정보를 담을 수 있다

```ts
type Someone = {
  author: {
    firstName: string;
    lastName: string;
  };
  name: string;
};

const someoneMatch: Someone = {
  author: {
    firstName: 'koby',
    lastName: 'lee',
  },
  name: 'Typescript',
};

const someoneMismatch: Someone = {
  author: {
    name: 'React',
    /**
  ❌ Error: Type '{ name: string; }' is not assignable 
  to type '{ firstName: string; lastName: string; }'.
  Object literal may only specify known properties, and 'name' 
  does not exist in type '{ firstName: string; lastName: string; }'.
 */
  },
  name: 'JavaScript',
};
```

객체의 속성으로 타입을 사용할 수 있기 때문에 위처럼 사용하는 것보다는 아래와 같이 사용하는 것이 가독성이 더 좋고, 오류 메세지도 보다 명확해진다.

```ts
type Someone = {
  firstName: string;
  lastName: string;
};
type doSomethong = {
  who: Someone;
  activity: string;
};
const someoneMismatch: doSomethong = {
  who: {
    name: 'leekoby',
    /**
          ❌ Error : Type '{ name: string; }' is not assignable to type 'Someone'.
          Object literal may only specify known properties, 
          and 'name' does not exist in type 'Someone'.
          */
  },
  activity: 'run',
};
```

</br>

## 4.2.4 선택적 속성

`?:`을 이용해서 선택적 속성을 부여할 수 있다.
선택적 속성 `?:` 대신 undefined를 사용할 수 있음
그러나 `undefined`를 유니언으로 선언했다면 속성 값을 반드시 선언한다.

```ts
type Someone = {
  name: string;
  age?: number;
  work: boolean | undefined;
};
const person1: Someone = {
  name: 'Aatrox',
  age: 26,
  work: false,
};
// ❌ Property 'work' is missing in type '{ name: string; age: number; }' but required in type 'Someone'.
const person2: Someone = {
  name: 'Aatrox',
  age: 26,
};
const person3: Someone = {
  name: 'Aatrox',
  work: false,
};
// `?:` 대신 undefined
const person4: Someone = {
  name: 'Aatrox',
  age: 26,
  work: undefined,
};
```

</br>

# 📖 4.3 객체 타입 유니언

- TS 코드에서는 속성이 조금 다른, 하나 이상의 서로 다른 객체 타입이 될 수 있는 타입을 설명할 수 있어야 한다.

- 속성값을 기반으로 해당 객체 타입 간에 타입을 좁혀야 할 수 도 있다.

</br>

## 4.3.1 유추된 객체 타입 유니언

- 변수에 여러 객체 타입 중 하나가 될 수 있는 초깃값이 주어지면 TS는 해당 타입을 객체 타입 유니언으로 유추한다.

- 유니언 타입은 가능한 각 객체 타입을 구성하고 있는 요소를 모두 가질 수 있다.
- 객체 타입에 정의된 각각의 가능한 속성은 비록 초깃값이 없는 선택적 타입이지만 각 객체 타입의 구성 요소로 주어진다.

```ts
const poem =
  Math.random() > 0.5 ? { name: 'leekoby', pages: 9 } : { name: 'kimcube', rhymes: true };
/*
type:
{
    name : string;
    pages : number;
    rhymes?: undefined;  
}
|
{
    name : string;
    pages ?: undefined;
    rhymes : boolean;  
}
 */
poem.name; //string
poem.pages; // number | undefined
poem.rhymes; // booleans | undefindes
```

다음 `poem` 값은 항상 `string` 타입인 `name`이고, `pages`와 `rhymes`는 있을 수도 있고 없을 수도 있다.

</br>

## 4.3.2 명시된 객체 타입 유니언 & 4.3.3 객체 타입 내로잉

- 객체 타입의 조합을 명시하면 객체 타입을 더 명확히 정의할 수 있다.
- 값이 타입이 객체 타입인 유니언이라면 일반적으로는 모든 유니언에 존재한느 값만 사용할 수 있다.
- 타입 내로잉을 통해서 타입을 좁혀서 좀 더 정확하게 타입을 추론하고 제어할 수 있다.

```ts
type Someone = {
  name: string;
  age: number;
};
type Stranger = {
  name: string;
  marriage: boolean;
};
let person: Someone | Stranger;
person = Math.random() > 0.5 ? { name: "leekoby", age: 33 } : { name: "kimcuby", marriage: true };
person.name;
person.age;
// ❌ Error: Property 'age' does not exist on type 'Someone | Stranger'.
//           Property 'age' does not exist on type 'Stranger'.

person.marriage;
// ❌ Error: Property 'marriage' does not exist on type 'Someone | Stranger'.
//           Property 'marriage' does not exist on type 'Someone'.
if ("age" in person) {
  person.age;
} else {
  person.marriage;
```

- 잠재적으로 존재하지 않는 객체의 멤버에 대한 접근을 제한하면 코드의 안전을 지킬 수 있다.
- 값이 여러 타입 중 하나일 경우, 모든 타입에 존재하지 않는 속성이 객체에 존재할 거라 보장 X
- 모든 타입에 존재하지 않은 속성에 접근하기 위해 타입을 좁혀야 하는 것처럼 객체 타입 유니언도 타입을 좁혀야한다.

- 타입 검사기가 유니언 타입 값에 특정 속성이 포함된 경우에만 코드 영역을 실행할 수 있음을 알게 되면, 값의 타입을 해당 속성을 포함하는 구성 요소로만 좁힌다.

- 코드에서 객체의 형태를 확인하고 타입 내로잉이 적용된다.

> Typescript는 `if(poem.pages)`와 같은 형식으로 참 여부 확인을 허용하지 않는다.

- 존재하지 않는 객체의 속성에 접근하려고 시도하면 타입 가드처럼 작동하는 방식으로 사용되더라도 타입 오류로 간주

```js
if (poem.pages) {
}
// ❌ Error : Property 'pages' does not exist on type 'Someone | Stranger'.
//            Property 'pages' does not exist on type 'Someone'.
```

</br>

</br>

## 4.3.4 판별된 유니언

- `Javascript`와 `Typescript`에서 유니언 타입으로 된 객체의 또 다른 인기 있는 형태는 객체의 속성이 객체의 형태를 나타내도록 하는 것, 이러한 형태를 `판별된 유니언` 이라고 한다.
- 객체의 타입을 가리키는 속성이 판별 값
- `Typescript`는 코드에서 판별 속성을 사용해 타입 내로잉 수행

- 판별된 유니온을 이용해서 타입 내로잉을 하는 것이 좋다.

- 판별된 유니온이란 각 객체마다 각자의 태그를 붙여서 해당 태그로 타입을 구분할 수 있도록 판별하는 것을 의미

```ts
type PoemPage = {
  name: string;
  pages: number;
  type: 'pages';
};

type PoemRhymes = {
  name: string;
  rhymes: boolean;
  type: 'rhymes';
};
type Poem = PoemPage | PoemRhymes;
const poem: Poem =
  Math.random() > 0.5
    ? { name: 'leekoby', pages: 9, type: 'pages' }
    : { name: 'kimcube', rhymes: true, type: 'rhymes' };

if (poem.type === 'pages') {
  console.log(`${poem.pages}`);
} else {
  console.log(`${poem.rhymes}`);
}

poem.type; // type : "pages" | "rhymes"
poem.pages;
// ❌ Error : Property 'pages' does not exist on type 'Poem'.
//         Property 'pages' does not exist on type 'PoemRhymes'.
```

</br>

# 📖 4.4 교차 타입

인터섹션(`&`)을 이용하여 각 타입에 선언된 모든 속성이 합집합인 타입이 된다

```ts
type Stranger1 = {
  name: string;
  age: number;
};
type Stranger2 = {
  name: string;
  marriage: boolean;
};

declare const person: Stranger1 & Stranger2;

/*
person ={
    name: string,
    age: number,
    marriage: boolean,
}
*/

person.name;
person.age;
person.marriage;
```

</br>

교차 타입과 판별된 유니온을 같이 사용한 예시

```ts
type Stranger1 = {
  age: number;
  type: 'stranger1';
};
type Stranger2 = {
  marriage: boolean;
  type: 'stranger2';
};

declare const person: { name: string } & (Stranger1 | Stranger2);

// "name: string"을 가지면서 "Stranger1"인 경우
if (person.type === 'stranger1') {
  person.age;
  person.name;
}
// "name: string"을 가지면서 "Stranger2"인 경우
else {
  person.name;
  person.marriage;
}
```

</br>

## 4.4.1 교차 타입의 위험성

교차 타입은 유용한 개념이지만, 혼동을 가져오기 쉽기 때문에 가능한 코드를 간결하게 유지해야한다.

> ### 🚨 교차 타입을 잘못 사용하면 불가능한 타입인 `never`가 생성될 위험이 있다.

- 교차 타입은 잘못 사용하기 쉽고 사용 불가능한 타입을 생성한다.
- 원시 값은 동시에 여러 타입이 될 수 없기에 교차 타입의 구성 요소로 함께 결합할 수 없다.
- 두 개의 원시 타입을 함께 시도하면 `never` 키워드로 표시되는 `never` 타입이 된다.
- `never`는 프로그래밍 언어에서 `bottom` 타입 또는 `empty` 타입을 뜻 한다.
- `bottom` 타입은 값을 가질 수 없고 참조할 수 없는 타입

```ts
// type SthType = never
type SthType = number & string;
```

대부분의 `Typescript` 프로젝트에서는 `never` 타입을 거의 사용하지 않지만 불가능한 상태를 나타내기 위해 가끔 사용한다.
</br>

# 📚 레퍼런스

> Goldberg, et al. 러닝 타입스크립트 / 조시 골드버그 지음 ; 고승원 옮김, 2023.

> <a href="https://typescript-kr.github.io/pages/unions-and-intersections.html" target="_blank"> Typescript 핸드북 </a>
