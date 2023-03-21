> 러닝타입스크립트 책에서 가져온 내용은 ""를 붙임

## Chapter 5. 함수

### 5.1 함수 매개변수

### 5.1.1 필수 매개변수

- required parameter
- "타입스크립트는 선언된 모든 매개변수가 필수라고 가정합니다."

<br>

### 5.1.2 선택적 매개변수

- "선택적 매개변수는 항상 undefined가 유니언 타입으로 추가되어 있습니다."
- "함수에서 사용되는 모든 선택적 매개변수는 마지막 매개변수여야 합니다."

```ts
function announceSong(song: string, singer?: string) {
  if (singer) {
    console.log(`Song: ${song}, Singer: ${singer}`)
  } else {
    console.log(`Song: ${song}`)
  }
}
announceSong('가나다라', 'Jpark')
announceSong('Yesterday')
```

<br>

### 5.1.3 기본 매개변수

- default parameter

```ts
function rateSong(song: string, rating = 0) {
  console.log(`${song} gets ${rating}/5 stars!`)
}

rateSong('가나다라') // default parameter는 optional로 가능
rateSong('Yesterday', 5)
rateSong('Yesterday', '5') // Argument of type 'string' is not assignable to parameter of type 'number'.
```

- 디폴트 파라미터를 사용한다고 하더라도, 타입을 선언할 때는 undefined도 같이 써야함

```ts
type MathFn = (a: number, b: number | undefined) => number

const sum: MathFn = (a, b = 0) => a + b
```

<br>

### 5.1.4 나머지 매개변수

### 5.2 반환 타입

- 함수는 반환 타입을 유추한다.

### 5.2.1 명시적 반환 타입

- 매개변수를 작성하고 ")" 다음에 타입 어노테이션으로 반환 타입 명시 가능

<br>

### 5.3 함수 타입

### 5.3.1 함수 타입 괄호

- 유니언 타입에 사용할 때 괄호로 묶는다

### 5.3.2 매개변수 타입 추론

### 5.3.3 함수 타입 별칭

### 5.4.1 void 반환 타입

- 어떤 값도 반환 X
- return 문이 없는 함수 또는 어떤 값도 반환하지 않는 return 문
- void 함수의 반환 타입이 무시된다는 것을 의미한다.
- undefined와 동일하지 않다.
- undefined는 반환되는 리터럴 값이다.
- undefined 대신 void 타입의 값을 할당하려고 하면 타입 오류가 발생

```ts
function add100(n: number): number {
  return n + 100
}

const addAndHandle = (n1: number, n2: number, callback: (num: number) => void): void => {
  return callback(n1 + n2)
}

const result = addAndHandle(10, 20, add100)
console.log(result) // 130, 왜 에러가 안나지?
```

<br>

### 5.4.2 never 반환 타입

- 아예 반환할 생각이 없다.
- 항상 오류를 발생시키거나 무한 루프를 실행하는 함수의 반환 타입으로 주로 사용

### 5.5 함수 오버로드

### 5.5.1 호출 시그니처 호환성

<br>

## Chapter 6. 배열

### 6.1 배열 타입

- `type[]`

<br>

### 6.1.1 배열과 함수 타입

```ts
let createStrings: () => string[]
let stringCreators: (() => string)[]
```

<br>

### 6.1.1 유니언 타입 배열

```ts
let stringOrArrayOfNumbers: string | number[]
let arrayOfStringOrNumbers: (string | number)[]
```

<br>

### 6.1.3 any 배열의 진화

- 배열도 초기에 타입을 정해주지 않으면 any[]로 시작해 진화한다.

<br>

### 6.1.4 다차원 배열

```ts
let arrayArray: number[][]
```

<br>

### 6.2 배열 멤버

### 6.2.1 주의 사항: 불안정한 멤버

- "때로는 값 타입에 대한 타입 시스템의 이해가 올바르지 않을 수 있습니다."
- "특히 배열은 타입 시스템에서 불안정한 소스입니다"

```ts
function withElements(elements: string[]) {
  console.log(elements[9001].length)
  // 타입 오류 없음
  // 런타입에서 오류 발생 Cannot read properties of undefined (reading 'length')
  // elements[9001]은 undefined가 아니라 string 타입으로 간주된다.
  //
}

withElements(['a', 'b'])
```

<br>

### 6.3 스프레드와 나머지 매개변수

### 6.3.2 나머지 매개변수 스프레드

### 6.4 튜플 Tuple

- 고정된 크기의 배열

```ts
let numAndStr: [number, string]

numAndStr = [1, '1']
numAndStr = [1, '1', 1]
// Type '[number, string, number]' is not assignable to type '[number, string]'.
// Source has 3 element(s) but target allows only 2
```

<br>

### 6.4.1 튜플 할당 가능성

- "가변 길이의 배열 타입은 튜플 타입에 할당할 수 없습니다."

<br>

### 나머지 매개변수로서의 튜플

- "타입스크립트는 나머지 매개변수로 전달된 튜플에 정호가한 타입 검사를 제공할 수 있습니다."

```ts
function logPair(name: string, value: number) {
  console.log(`${name} has ${value}`)
}

const pairArray = ['a', 1]
const Tuple1: [number, string] = [2, 'b']
const Tuple2: [string, number] = ['c', 3]

logPair(...pairArray) // A spread argument must either have a tuple type or be passed to a rest parameter.
logPair(...Tuple1) // Argument of type 'number' is not assignable to parameter of type 'string'.
logPair(...Tuple2)
```

<br>

### 6.4.2 튜플 추론

- "타입스크립트는 생성된 배열을 튜플이 아닌 가변 길이의 배열로 취급합니다."

#### 명시적 튜플 타입

- 타입 애너테이션 사용

#### const 어서션 (assortion)

- `as const` 연산자
- "타입스크립트에서 타입을 유추할 때 일긱 전용이 가능한 값 형식을 사용하도록 지시합니다."
- https://radlohead.gitbook.io/typescript-deep-dive/type-system/type-assertion
- 배열 리터럴 뒤에 적으면 튜플로 처리되야함을 나타낸다.

```ts
const unionArray = [100, '100'] // unionArray: (string | number)[]
const readonlyTuple = [100, '100'] as const // readonlyTuple: readonly [100, "100"]

unionArray[0] = '200'
readonlyTuple[0] = '200' // Error: Cannot assign to '0' because it is a read-only property.
```

<br>

## Chapter 7. 인터페이스 (Interface)

- "객체 형태를 설명하는 또 다른 방법"
- type alias와 유사하지만, "일반적으로 더 읽기 쉬운 오류 메시지, 더 빠른 컴파일러 성능, 클래스와의 더 나은 상호 운용성을 위해 선호됩니다."

<br>

### 7.1 타입 별칭 vs 인터페이스

```ts
type Poet = {
  born: number
  name: string
}

interface Poet {
  born: number
  name: string
}
```

- "인터페이스는 속성 증가를 위해 병합할 수 있습니다. 이 기능은 내장된 전역 인터페이스 또는 npm 패키지와 같은 외부 코드를 사용할 때 특히 유용합니다."
- "인터페이스는 클래스가 선언된 구조의 타입을 확인하는데 사용할 수 있지만 타입 별칭은 사용할 수 없습니다."
- "일반적으로 인터페이스에서 타입스크립트 타입 검사기가 더 빨리 작동합니다. 내부적으로 더 쉽게 캐시할 수 있는 명명된 타입을 선언합니다."
- "인터페이스는 이름 없는 객체 리터럴의 별칭이 아닌 이름 있는 객체로 간주되므로 어려운 특이 케이스에서 나타나는 오류 메시지를 더 쉽게 읽을 수 있습니다."

<br>

### 7.2 속성 타입 (속성들은 타입 별칭에서도 사용할 수 있다.)

### 7.2.1 선택적 속성

- "모든 객체가 필수적으로 인터페이스 속성을 가질 필요는 없습니다."
- 타입 애너테이션 앞에 `?` 사용

```ts
interface Book {
  author?: string // optional
  pages: number
}

const ok: Book = {
  author: 'AAA',
  pages: 80,
}

const missing: Book = {
  pages: 100,
}

const missingPages: Book = {
  author: 'BBB',
}
// Error: Property 'pages' is missing in type '{ author: string; }' but required in type 'Book'.
```

<br>

### 7.2.2 읽기 전용 속성

- "속성 이름 앞에 readonly 키워드를 추가해 다른 값으로 설정될 수 없음을 나타냅니다."
- 인터페이스에서만 사용할 수 있다

```ts
interface Book {
  author: string
  readonly pages: number
}

const bookA: Book = {
  author: 'AAA',
  pages: 80,
}

console.log(bookA.pages) // 80
bookA.pages = 10000 // Cannot assign to 'pages' because it is a read-only property.
```

<br>

### 7.2.3 함수와 메서드

### 7.2.4 호출 시그니처

- "인터페이스와 객체 타입은 호출 시그니처로 선언할 수 있습니다."
- "값을 함수처럼 호출하는 방식에 대한 타입 시스템의 설명입니다."

<br>

### 7.2.5 인덱스 시그니처

- 객체가 임의의 키를 받고, 해당 키 아래의 특정 탕비을 반환할 수 있음을 나타냅니다.
- 키/값을 저장하려고하는데 키를 미리 알 수 없다면 Map을 사용하는게 안전하다.

```ts
interface WordCounts {
  [i: string]: number // number 값을 갖는 모든 string 키를 허용한다.
}

const counts: WordCounts = {}

counts.apple = 0
counts.banana = 1
counts.cherry = '1' // Error: Type 'string' is not assignable to type 'number'.
```

<br>

### 속성과 인덱스 시그니처 혼합

```ts
interface WordCounts {
  test: number
  [i: string]: number
}

const counts: WordCounts = {
  aaaa: 100,
}
// Error: roperty 'test' is missing in type '{ aaaa: number; }' but required in type 'WordCounts'.(2741)
```

<br>

### 숫자 인덱스 시그니처

### 7.2.6 중첩 인터페이스

### 7.3 인터페이스 확장

- `extends` 키워드 사용
- class와 유사하다.

### 7.3.1 재정의된 속성

- 인터페이스의 속성을 override 할 수 있다.

```ts
interface WithNullableName {
  name: string | null
}

interface WithNonNullableName extends WithNullableName {
  name: string // override로 타입이 string으로 좁혀짐
}

interface WithNumbericName extends WithNonNullableName {
  name: number | string
  // 좁혀진 타입은 다시 확장 불가 Error: Interface 'WithNumbericName' incorrectly extends interface 'WithNonNullableName'.
}
```

### 7.3.2 다중 인터페이스 확장

- 여러 개의 다른 인터페이스를 동시에 확장할 수 있음

```ts
interface A {
  a(): string
}

interface B {
  b(): number
}

interface C extends A, B {
  c(): boolean
}

function ABC(abc: C) {
  abc.a()
  abc.b()
  abc.c()
}
```

<br>

### 7.4 인터페이스 병합

- "두 개의 인터페이스가 동일한 이름으로 동일한 스코프에 선언된 경우, 선언된 모든 필드를 포함하는 더 큰 인터페이스가 코드에 추가됩니다."
- 가능하면 사용하지 않는게 좋다.

```js
interface AB {
  a(): string;
}

interface AB {
  b(): number;
}

// 아래와 같음
// interface AB {
//   a():string
//   b():number
// }
```

<br>

### 7.4.1 이름이 충돌되는 멤버

- "병합된 인터페이스는 타입이 다른 동일한 이름의 속성을 여러 번 선언할 수 없습니다."
- "속성이 이미 인터페이스에 선언되어 있다면 나중에 병합된 인터페이스에서도 동일한 타입을 사용해야 합니다."
