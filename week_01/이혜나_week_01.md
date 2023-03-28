### 타입의 종류
: 타입이란 자바스크립트에서 다루는 값의 형태에 대한 설명이다.
(형태란, 값에 존재하는 속성과 메서드 그리고 내장되어 있는 typeof 연산자가 설명하는 것을 의미)


### - 일곱가지의 원시타입
1. `null`
2. `undefined`
3. `boolean`
4. `string`
5. `number`
6. `bigint`
7. `symbol`


### 타입 시스템
- 코드를 읽고 존재하는 모든 타입과 값을 이해한다.
- 각 값이 초기 선언에서 가질 수 있는 타입을 확인한다.
- 각 값이 추후 코드에서 어떻게 사용될 수 있는지 모든 방법을 확인한다.
- 값의 사용법이 타입과 일치하지 않으면 사용자에게 오류를 표시한다.

```typescript
// string
let firstName = "Lee";

// number
let age = 22;

// string | number
let total = Math.random() > 0.5 ? "string" : 22
```


### 오류 종류

1. 구문 오류
> 타입스크립트가 코드로 이해할 수 없는 잘못된 구문을 감지할 때 발생한다. 이는 타입스크립트가 타입스크립트 파일에서 자바스크립트 파일을 올바르게 생성할 수 없도록 차단합니다.
``` typescript
let let bts;
// Error: ',' 가 필요!!

```


2. 타입 오류
> 타입스크립트의 타입 검사기가 프로그램의 타입에서 오류를 감지했을 때 발생한다. 오류가 발생했다고 해서 타입스크립트 구문이 자바스크립트로 변환되는 것을 차단하지 않습니다.

``` typescript
console.blub("Hello world");
// Error : Property "blub" does not exist on type "Console" 
```


### 할당 가능성
: 타입스크립트는 변수의 초깃값을 읽고 해당 변수가 혀용되는 타입을 결정합니다. 이전에 변수에 새로운 값이 할당되면, 새롭게 할당된 값의 타입이 변수의 타입과 동일한지 확인합니다.
즉, 함수 호출이나 변수에 값을 제공할 수 있는지 여부를 확인하는 것을 할당 가능성 이라고 합니다.

``` typescript
// string
let firstName = "Lee";
firstName = "banana";

// number
let age = 5;
age = 25; 

let lastName = "hazel";
lastName = true;
// Error : type "boolean" is not assignable to type "string".

```



### 타입 애너테이션(Type annotation)
: 초깃값이 없는 경우라면 타입 애너테이션을 이용해 타입을 지정할 수 있다.
초기 타입을 유츄할 수 없는 변수는 `진화하는 any` 라고 부릅니다.

``` typescript
let myName: string;
myName = "hazel";
myName = 22; // Error : "number"은 "string" 형식에 할당 할 수 없다.

```


초깃값을 할당하지 않고도 변수의 타입을 선언할 수 있는 구문을 `type annotation`을  제공한다. 변수 이름뒤에 배치되며 콜론(:) 과 타입 이름을 차례대로 기재하면 된다.


### 타입 형태 
: 변수에 할당된 값이 원래 타입과 일치하는지 확인하는 것 이상을 수행한다.
``` typescript
let cher = {
  firstName: "lee",
  lastName: "hazel",
};

cher.middleName;
// Error: Property "middleName" does not exist on type
// { firstName : stirng; lastName: string; }
```


### 모듈과 스크립트
: ECMA 스크립트 2015에는 파일 간에 가져오고 내보내는 구분을 표준화 하기 위해 ECMA 스크립트 모듈 ESM 사용한다.
참고로, 모듈 파일은 ./values 파일에서 value 를 가져오고, 변수 doubled를 내보낸다.

>import {value} from "./values";
export const doubled = value * 2;


- 모듈 : `export | import`가 있는 파일
- 스크립트 : 모듈이 아닌 모든 파일 


1. 모듈
: 모듈 파일에 선언된 모든 것은 해당 파일에서 명시한 export 문에서 내보내지 않는 한 모듈 파일에서만 사용할 수 있다. 한 모듈에서 다른 파일에 선언된 변수와 동일한 이름으로 선언된 변수는 다른 파일의 변수를 가져오지 않는 한 이름 충돌로 간주 하지 않는다.
```typescript
// a.ts
export const myName = "Lee";


// b.ts
expoprt const myName = "Lee";

// c.ts
import { myName } from "./a";
console.log(myName);
```



2. 스크립트
: 파일이 스크립트라면 해당 파일을 전역 스코프로 간주하므로 모든 스크리트가 파일의 내용에 접근 할 수 있다. 즉 스크립트 파일에 선언된 변수는 다른 스크립트 파일에 선언된 동일하 이름을 가질 수 없다.
``` typescript
// a.ts
const myName = "Lee"
// Error : Cannot redeclare blok-scoped varible "myName".

// b.ts
const myName = "Lee";
// Error : Cannot redeclare block-scoped varible "myName".

// a.ts && b.ts
const myName = "Lee";  
export {};
```



[Reference]
1. Learning typeScript(조시 골드버그/고승원/한빛미디어_2023)


### 유니언(Union)
- 값에 허용된 타입을 두 개 이상의 가능한 타입으로 확장하는 것
- "이거 혹은 저거" 같은 타입을 `Union` 이라고 함
- 유니언을 선언한 모든 가능한 타입에 존재하는 멤버 속성에만 접근 할 수 있다.

``` typescript
// sring | number
let bts1: string | number;
let bts2 = Math.random() > 0.5 ? taetae : 29;
bts2.toString();   // ok
bts2.toFixed();  // Error: property "toFixed" does not exist on type 'string |number'.

```


### 내로잉(Narrowing)
- 값에 허용된 타입이 하나 이상의 가능한 타입이 되지 않도록 좁히는 것!!
- 타입을 좁히는데 사용할 수 있는 논리적 검사를 `타입 가드` 라고 한다.

#### 1. 값 할당을 통한 내로잉
```typescript
let animal = number | string;
animal = "rabbit";
animal.toUpperCase();   // ok: stirng
animal.toFixed();
// Error: property 'toFixed' does not exist on type 'string'

```
변수에 유니언 타입 애너테이션이 명시되고 초깃값이 주어질 때 값 할당 내로딩이 작동된다. 타입스크립트는 변수가 나중에 유니언 타입으로 선언된 타입 중 하나의 값을 받을 수 있지만, 처음에는 할당된 값의 타입으로 시작한다는 것을 이해헤야한다.


#### 2.  조건 검사를 통한 내로잉
- if 문을 통해 변수의 값을 좁히는 방법을 사용한다.
```typescript
let bts = Math.randon() > 0.5 ? "taetae" : 29;

if(bts === "taetae") {
  // bts. string 타입
  bts.toUpperCase();   // ok
}


// bts : number | string 타입
bts.toUpperCase();

// Error : Property 'toUpperCase" does not exist on type 'string| number'
// Property 'toUpperCase' does not exist on type 'number'

```


#### 3. typeof 검사를 통한 내로잉
-  직접 값을 확인해 타입을 좁히기도 하지만, typeof 연사자를 사용할 수 있다.

```typescript
let bts = Math.randon() > 0.5 ? "taetae" : 29;

bts.toString();

if(typeof bts === "string") {
  // string
  bts.toUpperCase();
if(typeof bts === "number") {
  // number
  bts.toFixed();
```

### 리터럴 타입 (Literal type)
- 원시 타입 값 중 어떤 것이 아닌 특정 원시값으로 알려진 타입이 `리터럴 타입`이라고 한다.
- 원시 타입 string 은 존재 할 수 있는 모든 가능한 문자열의 집합을 나타낸다.
- 즉, 리터럴 타입인 "coughing" 는 하나의 문자열만 나타낸다.

```typescript
const myState = "coughing" 

```

1.리터럴 할당 가능성
: number와 string 과 같은 서로 다른 원시 타입이 서로 할당되지 못한다.
: 즉 0과 1 처럼 동일한 원시타입일지라도 서로 다른 리터럴 타입은 서로 할당 할 수 없다.

```typescript
let bts:"Taetae" = "Taetae;
bts = "Blackpink"; 		// Error : Type "Blackpink" is not assignable to type "Taetae".


let someSong: "";
bts = someSong;
// Error : type 'string' is not assignable to type 'Taetae'.
```


### 엄격한 null 검사
: null과 undefined를 활성화 할 지 여부 결정하는 것.
- strictNullCecks는 엄격한 null 검사를 활성화 할지 여부 결정하고 strictNullChecks를 비활성하면 코드의 모든 타입에 | null | undefined를 추가해야 모든 변수가 null | undefined를 할당한다.

```typescript
let myName: string;

// strictNullChecks : false
myName = null;  // ok

// strictNullChecks : true
myNamn = null;  // Error : Type. 'null' is not assignable to type 'string'.

```

1. 참 검사를 통한 내로잉
- 참 | truthy 는 && 연사자 | if 문처럼 boolean 문맥에서 true 간주.
- false, 0, -0, on, "", null, undefined, NaN 처럼 falsy 로 정의된 값을 제외한 모든 값은 모두 참이다.
``` typescript
let breakfast = Math.random() > 0.5 ? "McDonald" : undefined;
	if(breakfast) {
  		breakfast.toUpperCase();   // ok
    breakfast.toUpperCase();
  // Error : Object is possibly 'undefined'
      
 
      

breakfast && breakfast.toUpperCase();    // ok: string | undefined
breakfast?.toUpperCase();

```
``` typescript
let weekend = Math.random() > 0.5 && "Play Catch Ball";
if(weekend) {
  weekend;   // type: string
} else {
 	weekend;  // type: false | string
}
```

2. 초깃값이 없는 변수
- 초깃값이 없는 변수는 기본적으로 undefined
- 값이 할당될 때까지 변수가 undefined 임을 이해하함.
``` typescript
let weekend: string;
weekend?.length;
// Error :  Variable 'weekend' is used before being assingend.

weekend = "Play Catch Ball";
weekend.length;  // ok
```


타입 별칭
: type 키워드를 이용해서 타입에 대한 변수를 생성한다.
- 재사용하는 타입에 더 쉬운 이름을 할당하는 `타입별칭` dla
- type 새로운 이른 = 타입 형태를 갖는다. (편의상 파스칼 케이스 이름으로 지정)
- 타입 별칭은 "복사해서 붙여넣기" 작동
- 타입 별칭은 자바스크립트에 존재 하지 않는다
``` typescript
type HyungGu = ..; 
```

```typescript 
type HyungGu = boolean | number | string | null | undefinedl
let HyunGuFirst: HyunGu;
let HyunGuSecond: HyunGu;
let HyunGuThird: HyunGu;

```

[Reference]
1. Learning typeScript(조시 골드버그/고승원/한빛미디어_2023)

### 객체 타입
- {...} 구문을 사용해서 객체 리터럴을 생성
- 값의 속성에 접근하려면 value.멤버 | value['멤버']
```typescript
let spring = {
	flower: "Cherry Blossom",
  	month: 4
};
spring['flower'];   // type: string
spring.month;   	// type: number

spring.end;
// Error : Property 'end' does not exist on type '{ flower: string; month: number; }`.
```

#### 1.객체 타입 선언
``` typescript
let myState: {
	name: string;
    age: number;
};

// ok
myState = {
  name: "hazel",
  age: 22,
};
```


#### 별칭 객체 타입
``` typescript
let MyState: {
	name: string;
    age: number;
};

let myState: MyState;

myState = {
  name: "hazel",
  age: 22,
};
```


### 구조적 타이핑
- 타입을 충족하는 모든 값을 해당 타입의 값으로 사용할 수 있다.
- 매개변수나 변수가 특정 객체 타입으로 선언되면 타입스크립트에 어떤 객체를 사용하든 해당 속성이 있어야 한다.
```typescript
type FirstNmae = { firstName: string };
type LastName = { lastName: string };

const fullName = {
  firstName: "Lee",
  lastName: "hazel",
};

// firstName
let firstName: FirstName = fullName;
// lastName
let lastName: LastName = fullName;
```

**구조적 타이핑은 덕 타이핑과는 다르다.**
- 타입스크립트의 타입 검사기에서 구조적 타이핑은 정정 시스템이 타입을 검사하는 경우이다.
- 덕 타이핑은 런타입에서 사용될 때까지 객체 타입을 검사하지 않는 것을 말한다.
=> 자바스크립트 덕타입인 반면 타입스크립트는 구조적으로 타입화 된다.


### 1.사용 검사
1) 객체 타입으로 값을 해당 객체 타입에 할당 할 수 있는지 확인
2) 할당하는 값에는 객체 타입의 필수 속성이 있어야 한다.
- 객체 타입은 필수 속성 이름과 해당 속성이 예상되는 타입을 모두 지정.
- 일치하지 않으면 타입스크립트는 타입 오류 발생
```typescript
type FirstAndLastNames = {
  first: string;
  last: string;
};

const hasBoth: FirstAndLastNames = {
  first: "Lee",
  last: "hazel",
};

const hasOnlyOne: FirstAndLastNames = {
  // Error : Property 'last' is missing in type '{ first: string }'
  // but required in type 'FirstAndNames'.
  first: "Lee"
};
```


### 2. 초과 속성 검사
- 변수가 객체 타입으로 선언하고, 초깃값에 객체 타입에서 정의된 것보다 많은 필드가 있다면 타입스크립트에서 타입 오류가 발생한다.
``` typescript 
type Person = {
  born: number;
  name: string;
}

// ok: Person의 필드와 일치함
const person1: Person = {
  born: 2007,
  name: "Anna"
};

type Author = {
  firtsName: string;
  lastName: string;
};

type Poem = {
	author: Author;
  	name: string;
};

const poemMismatch: Poem = {
  author: {
    name: "hazel",
    //Error: Type { name : string } is not assignable to type 'Author'.
    // Object literal may only specify known properties.
    // and 'name' does not exist in type 'Author'.
  },
name: "Tulips",
};
```

### 3. 선택적 송성
- 중첩된 객체 타입을 고유한 이름으로 바꿔서 사용하면 코드와 오류 메시지가 더 읽기 쉽다.
- 타입의 속성 애너테이션에서 : 앞에 ? 를 추가하면 선택적 속성임을 나타낼수 있다.
- 선택적 속성은 존재하지 않아도 되고 undefined를 넣어줘도 된다.
- 즉, undefinex를 유니언으로 선언시 속성 값을 반드시 선언해줘야 한다.

```typescript 
type Writers = {
  author: string | undefined;
  editor?: string;
};

// ok : author 는 undefined으로 제공
const hasRequired: Writers = {
  author: undefined,
},
      
const missingRequried: Writers = {};

// ERROR : Property 'author' is missing in type {} but required in type 'Writers'.
```


### 객체 타입 유니언
#### 1. 유추된 객체 타입 유니언
```typescript
const bts = Math.random() > 0.5
? { name: "taetae", age:29}
: { name: "RM", gender: true};

// type:
{
  name: string;
  age: number;
  gender?: undefined;
} | {
  name: string;
  age: undefined;
  gender: boolean;
}
bts.name;   //string
bts.agel;   // number | undefined
bts.gender;   // boolean | undefined
```



2. 명시된 객체 타입 유니언
- 값의 타입이 객체 타입으로 구성된 유니언이라며, 이런 모든 유니언 타입에 존재 하는 속성에 대한 접근만 허용한다.
- 속성 name에 접근하는 것은 name 속성이 항상 존재하기 때문에 허용되지만 age와 gender 는 항상 존재한다는 보장이 없다.
```typescript
type Person1 = {
  name: string;
  age: number;
};

type Person2 = {
  name: string;
  gender: boolean;
};

let pernson: Person1 | Person2;

const personL Person = Math.random() > 0.5 
	? { name: "taetae", age: 29} 
 	: { name: "RM", gender: true};

person.name;
person.age;  // Error : Property 'age' does not exist on type "Person1 | Person 2'.
person.gender; 
// Error : Property 'gender' does not exist on type "Person1 | Person 2'.

if('age' in person) {
  person.age;
} else {
  person.gender;
}
  
```

### 교차 타입
- & 교차타입(intersection type) 사용해 여러 타입을 동시에 나타낸다.
- 일반적으로 여러 기존 객체 타입을 별칭 객체 타입으로 결합해 새로운 타입을 생성.
```typescript
type Person1 = {
	name: string'
  	age: number;
};
type Person2 = {
  name: string;
  gender: boolean;
};

type Persons = Person1 & Person2;
//
{
  person.name;
  person.age;
  person.gendr;
}

```

#### 1. 교차 타입의 위험성
- 긴 할당 가능성 오류 : 복잡할수록 타입 검사기의 메시지도 이해하기 더 어렵다. 
- 타입의 일련의 별칭으로 된 객체 타입으로 분할하면 읽기가 훨씬 쉬워진다.
```typescript
type ShortPoemBase = { author: string};
type Haiku = ShortPoemBase & { kigo: string; type: "haiku" };
type villanelle = ShortPoemBase & { meter: number; type: "villanelle" };
type ShortPoem = Haiku | Villanelle;

const oneArt: ShortPoem = {
  
 // Error : type ' { author: string; type: "villanlle"; }'
  // is  not assignable to type 'ShortPoem'
  // type '{author: string; type: "villanlle";}' is not assignable to type 'villanelle'.
  // Property 'meter' is missing in type '{ author: string; type: "villanelle"}
  // but required in type '{meter: numberl type: 'villanelle'}
author: "Elizabeth Bishop",
type": 'villanelle',
};

```

1-1 never
- 교차 타입은 잘못 사용하기 쉽고 불가능한 타입을 생성한다.
- 두 개의 원시 타입을 함께 시도하면 never 키워드로 표시되는 never 타입이 된다.
``` typescript
type MySpring = number & string;   // type: never

```


[Reference]
1. Learning typeScript(조시 골드버그/고승원/한빛미디어_2023)

