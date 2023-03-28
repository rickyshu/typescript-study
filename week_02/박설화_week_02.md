[[러닝타입스크립트]Part2. 특징 - chapter5. 함수](https://velog.io/@iberis/러닝타입스크립트Part2.-특징-chapter5.-함수)\
[[러닝타입스크립트] Part2. 특징chapter6. 배열](https://velog.io/@iberis/러닝타입스크립트)\
[[러닝타입스크립트]Part2. 특징 - chapter7. 인터페이스](https://velog.io/@iberis/러닝타입스크립트Part2.-특징-chapter7.-인터페이스)

## 🐰 배열

- 자바스크립트는 배열 내부에 모든 타입의 값을 혼합해서 저장할 수 있지만,\
  타입스크립트는 초기 배열 요소의 타입을 기억하고, 해당 타입에서만 작동하도록 제한한다.
  - 빈 배열을 선언하면서 요소에 아무 타입도 지정하지 않으면 `:any[]`로 취급하여 모든 타입의 요소를 받을 수 있다.
- 배열 요소 타입 다음 `[]` 를 기재한다.
- `Array<타입>` 으로 작성할 수도 있다.

```ts
/* 빈배열의 타입을 지정하지 않은 경우 */
let anyAry = [];
anyAry.push("가나다");
anyAry[0] = 10;
console.log(anyAry); // [10]

/* 초깃값이 있는 경우 */
const number = [0, 1, 2, 3, 4, 5];
number.push("가나다");
// Argument of type 'string' is not assignable to parameter of type 'number'.

/* 초깃값을 지정하지 않아도 undefined로 시작해서 나중에 배열 값을 받을 수 있다. */
const stringAry: string[];
stringAry = ["가", "나", "다"];

/* Array<타입유형> */
const stringAry2: Array<number> = [1, 2, 3, 4, 5];
```

배열의 요소가 함수 또는 유니언 타입인 경우 `()` 괄호로 명확히 구분해주어야 한다.

```ts
🍀 다음 밑줄친 빈 칸에 적절한 타입은❓ (복수 정답도 있음)

let X타입: () => string[];
let Y타입: (() => string)[];

// ____ = () => ['hello', 'world'];
// ___ = [function(){return '안녕'}, () => '세상']

let A번타입: string | number[];
let B번타입: (string | number)[];

// _____ = [1, 2, 3]
// _____ = ['가']
// _____ = [0, '가']
// _____ = '가나다'
```

```
정답
X타입
Y타입
A번타입, B번타입 둘 다
B번타입
B번타입
A번타입
```

### 🥕 다차원 배열

`[]` 대괄호를 중첩하여 사용한다.

```ts
let aryOfAry: number[][];
aryOfAry = [
  [1, 2, 3],
  [4, 5, 6],
];

/* 🍀 다음 밑줄친 배열의 타입 애너테이션은❓ */
let 다중배열: ________;
다중배열 = [
  [1, 2, 3],
  [4, [5, 6]],
];
```

```
정답
let 다중배열: (number | (number)[ ])[ ][ ];
```

### 🥕 배열 멤버

유니언 타입으로 된 베열의 요소는 그 자체로 동일한 유니언 타입이다.

```ts
const friendInfo = ["Hani", 30, new Date(1993, 3, 1)];

// age는 string | number | Date 타입이다.
let age = friendInfo[1];
age = "30살";
```

불안정한 멤버

```ts
function a(b: string[]) {
  console.log(b[100].length); // 타입 오류 없음
}

a(["가나다"]);
// [ERR]: undefined is not an object (evaluating 'b[100].length')
```

코드를 실행시키면 에러가 발생하지만,
타입스크립트 컴파일러의 기본 설정에서 오류를 표시하지 않는다.

### 🥕 ...Spread , Rest 파라미터

```ts
/* 스프레드로 합친 배열은 각 배열의 타입들을 모두 포함한다. */
const stringAry = ["Harry", "Joanna", "kai"];
const numAry = [20, 19, 27];

// unionAry의 타입은 (string : number)[]
const unionAry = [...stringAry, ...numAry];

/* ...rest 파라미터도 지정된 타입의 요소만 받는다. */
function greetingFunc(greeting: string, ...names: string[]) {
  for (const name of names) {
    console.log(`${greeting}, ${name}`);
  }
}

// string 타입의 요소를 갖는 배열은 아규먼트(매개변수)로 전달할 수 있다.
const friends = ["Anna", "Joa", "Jini"];
greetingFunc("Hello", ...friends);

// string 타입이 아닌 요소의 배열은 타입 에러가 난다.
const age = [21, 23, 27];
greetingFunc("my age is", ...age);
// Argument of type 'number' is not assignable to parameter of type 'string'.
```

### 🥕 튜플

튜플 배열은 크기(length)와 각 요소의 타입이 고정되어 있다.

- `[ ]` 대괄호 안에 해당 인덱스 요소의 타입을 적는다.

```ts
/* 0번 인덱스 요소는 number 타입, 
1번 인덱스 요소는 string 타입을 갖는다. */
let tuple1: [number, string];

tuple1 = [123, "가나다"];

/* 크기(length)가 2로 고정되어 있다. */
tuple1 = [123];
// Type '[number]' is not assignable to type '[number, string]'.
//  Source has 1 element(s) but target requires 2.

/* 각 인덱스의 요소 마다 타입이 고정되어 있다. */
tuple1 = ["가나다", 123];
// Type 'string' is not assignable to type 'number'.
// Type 'number' is not assignable to type 'string'.

/* 초깃값을 설정하며 구조 분해 할당도 가능하다 */
/* name1은 string 타입, age는 number 타입이다. */
let [name1, age] = ["Minji", 29];
name1 = 299;
// Type 'number' is not assignable to type 'string'.
```

가변 길이의 배열 타입은 튜플 타입에 할당할 수 없다.

```ts
/* 🍀 notTuple의 타입은❓ */
const notTuple = [true, 123];

/* 🍀 tuple2에 notTuple을 할당할 수 있나요❓ */
let tuple2: [boolean, number] = notTuple;

const tuple3: [booleam, number, string] = [false, 100, "abc"];
/* 🍀 tuple2에 tuple3을 할당할 수 있나요❓ */
tuple2 = tuple3;
```

```
정답
(boolean | number)[ ]

할당할 수 없다.
```

```ts
let tuple2: [boolean, number] = notTuple;
// Type '(number | boolean)[]' is not assignable to type '[boolean, number]'.
//Target requires 2 element(s) but source may have fewer.
```

```
할당할 수 없다.
```

```ts
let tuple2: [boolean, number];
const tuple3: [boolean, number, string] = [false, 100, "abc"];
tuple2 = tuple3;
// Type '[boolean, number, string]' is not assignable to type '[boolean, number]'.
//  Source has 3 element(s) but target allows only 2.
```

### 🥕 Rest 파라미터와 튜플

```ts
/* 🍀...rest 파리미터를 이용해 함수를 호출할 때, 다음 타입 에러를 고치려면 friend1에 무엇을 추가하면 될까요❓ */

function introduce(name: string, age: number) {
  console.log(`${name} is ${age}.`);
}

const friend1 = ["Tina", 10];
introduce(...friend1);
// A spread argument must either have a tuple type or be passed to a rest parameter.
```

```ts
정답;
const friend1: [string, number] = ["Tina", 10];
introduce(...friend2);
```

함수에 여러 아규먼트를 전달할 때, 전달할 아규먼트들을 하나의 튜플 배열로 담아 forEach와 ...rest 파라미터를 이용할 수 있다.

```ts
function grade(subject: string, score: [number, boolean]) {
  console.log(`${subject} is ${score}`);
}

const middleExam: [string, [number, boolean]][] = [
  ["English", [100, true]],
  ["Math", [90, true]],
  ["computer", [50, false]],
];

middleExam.forEach((sub) => grade(...sub));
```

타입스크립트는 배열이 **변수의 초깃값** 또는 **함수에 대한 반환값**으로 사용되는 경우, 고정된 크기의 튜플이 아닌 **유연한 크기의 배열**로 가정한다.

튜플 타입을 반환하기 위해서는 명시적으로 타입 애너테이션을 사용해야한다.

```ts
function aryFunc(input: string) {
  return [input[0], input.length];
}

let [alphabet, size] = aryFunc("abc");
// alphabet과 size의 타입은 모두 string | number 이다.

/* 튜플 타입 애너테이션을 작성한 경우 */
function tupleFunc(input: string): [string, number] {
  return [input[0], input.length];
}

[alphabet, size] = tupleFunc("abc");
// alphabet 는 string 타입
// size는 number 타입이다.
```

### 🥕 const assertion

const assertion은 배열 리터럴 뒤에 `as const` 를 작성하여 표시한다.

- 타입을 읽기 전용(**read only**)이고, 요소의 값 수정이 불가하다.
- 배열이 튜플로 처리되어야 함을 나타낸다.

```ts
// 🍀 빈 칸에 들어갈 첫 번째 요소를 문자열로 바꿀 수 있는 배열은 둘 중 어떤 걸까요❓
const aryA = [100, "abc"];
const aryB = [100, "abc"] as const;

____[0] = "가나다";
```

```
정답
aryA
```

aryA의 타입은 `(string | number)[]` 이고,\
aryB의 타입은 `readonly [100, 'abc']` 이다.\
따라서 아래와 같이 사용할 수 없다.

```ts
const aryB: [number, string] = [100, "abc"] as const;
// The type 'readonly [100, "abc"]' is 'readonly' and cannot be assigned to the mutable type '[number, string]'.
```

읽기 전용 튜플 함수
반환 값에 `as const`를 붙여 읽기 전용 튜플 함수를 만들 수 있다.

```ts
// 반환 타입 readonly[string, number]
function readOnlyFunc(input: string) {
  return [input[0], input.length] as const;
}

// alphabet의 타입은 string 이고, size의 타입은 number이다.
let [alphabet, size] = readOnlyFunc("Hello");

// 🍀 alphabet 변수에 string 타입 또는 number 타입의 다른 값을 할당할 수 있을까요❓
alphabet = "hi";
alphabet = 10;
```

```
정답
string 타입의 다른 값은 할당할 수 있다.
number 타입의 값은 할당할 수 없다.
```

```ts
alphabet = 10;
// Type 'number' is not assignable to type 'string'.
```
