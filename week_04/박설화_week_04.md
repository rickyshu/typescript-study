# 🧬 제네릭(Generics)

## [공식문서 핸드북 : Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

> 과도한 제내릭은 코드를 읽기 어렵게 만드므로, 필요할 때만 사용하고 무엇을 위해 사용하는지 명확히 하는 것이 중요하다.
>
> - 타입 매개변수가 최소 2번 이상 사용되는 곳에서 사용해야 한다.
> - 타입 배개변수에 대한 표준 명명 규칙 'T', 'U', 'V' 로 사용한다.
> - 그러나 약어 대신 파스칼 케이스로 완전한 이름을 사용하는 것이 더 좋다.

제네릭을 사용하면 함께 작동하는 데이터 구조를 만들거나 다양한 타입의 값을 래핑할 수 있습니다(예: 모든 타입의 데이터를 저장할 수 있는 배열).

```ts
const names: Array<string> = []; // 아래와 동일하다.
const names2: string[] = [];
```

제네릭은 메인 타입에 연결된 부가 타입에 대한 정보를 구체화준다.

- 배열은 배열의 요소로 어떤 타입이 들어갈 지 알려주며,
- Promise 타입은 어떤 타입을 반환할 지 알려준다.

제네릭을 지정해주지 않으면 에러가 나며, \
구체적으로 지정하지 않고 `<any>`로 두는 경우 타입 에러는 일어나지 않지만, 타입검사기가 제대로 작동하지 않는다.

```ts
const promise: Promise<any> = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(100);
  }, 1000);
});

// 숫자를 반환하지만, 문자열 메서드를 사용해도 타입 에러로 알려주지 않는다.
promise.then((data) => data.split(""));
```

## 🧞‍♀️ 제네릭 함수

두 객체를 합치는 함수를 만들면서 `object` 타입만 지정해주면 반환값의 타입도 어떠한 속성도 들어있지 않은 `{}` 타입이 된다. \
따라서 각 파라미터에 들어있던 프로퍼티를 사용할 수 없다.

```ts
/* 제네릭을 사용하지 않은 경우 */
function merge(objA: object, objB: object) {
  return { ...objA, ...objB };
}

const mergedObj = merge({ name: "Max" }, { age: 30 });
console.log(mergedObj.age); // Property 'age' does not exist on type '{}'.

// 단, const mergedObj = merge({ name: "Max" }, { age: 30 }) as { name: string, age: number}
// 이렇게 지정해줄 수 있지만, 함수를 호출할 때마다 지정하기가 매우 번거롭다.

/* 제네릭을 사용한 경우 */
function merge<T, U>(objA: T, objB: U) {
  return { ...objA, ...objB };
}

const mergedObj = merge({ name: "Max", hobbies: ["Sports"] }, { age: 30 });
console.log(mergedObj.age); // 30

/* 화살표 함수 제네릭 */
const marge2 =  <T, U> (objA: T, objB: U) => { ...objA, ...objB };
```

`merge()` 함수의 파라미터의 각각의 타입을 **T**, **U**로 저장해서, `merge()` 함수에서 **T**, **U**타입을 사용했다는 것을 구체화할 수 있다.

이렇게 `merge()` 함수로 만들어진 `mergedObj`에는 **T**, **U** 타입 모두가 들어있으므로,
각 타입에 있는 프로퍼티를 사용할 수 있다.

## 🧞‍♀️ 제네릭 키워드

### 🍇 제네릭 기본값 `=`

기본값은 타입이 명시적으로 제공되지 않은 경우 사용된다.

```ts
interface Book<P, B = string> {
  popular: P; // 기본 타입 없음
  title: B; // 기본 타입 string
}

let HarryPotter1: Book<boolean, string> = {
  popular: true,
  title: "마법사의 돌",
};

let HarryPotter2: Book<boolean> = {
  popular: true,
  title: "비밀의 방", // 기본값으로 string 타입이 지정됐다.
};

let HarryPotter3: Book<boolean> = {
  popular: true,
  title: 100, // Type 'number' is not assignable to type 'string'.
};

let HarryPotter4: Book = {
  // Generic type 'Book<P, B>' requires between 1 and 2 type arguments.
  popular: true,
  title: "불의 잔",
};
```

제네릭 타입이 여러개인 경우, 가장 마지막에 와야한다.

- 기본값이 없는 제네릭 타입은 기본값이 있는 제네릭 타입 뒤에 오면 안된다.

### 🍇 extends

제네릭에 `extends` 키워드로 구체적으로 어떤 타입이 들어와야하는지도 지정해줄 수 있다.

```ts
function merge<T extends object, U extends object>(objA: T, objB: U) {
  return Object.assign(objA, objB); // objA와 objB가 모두 객체 타입이므로 객체 메서드를 사용할 수 있다.
}

const mergedObj = merge({ name: "Max", hobbies: ["Sports"] }, { age: 30 });
console.log(mergedObj.age); // 30
```

`<T extends object, U extends object>` 로 지정하면 **T, U** 타입에는 **객체(object) 타입만** 들어와야 한다.
따라서 객체의 내장 메서드도 사용할 수 있다.
객체 외 다른 타입이 들어올 경우 타입스크립트가 에러를 표시해준다.

#### 📌 특정 속성을 가진 여러 타입

기본 타입 외에도 `interface` 로 특정 속성을 지닌 타입이면 모두 가능하도록 정해줄 수도 있다.

```ts
interface Length {
  length: number; // length 프로퍼티를 가지고 있는 타입
}

// 파라미터와 문자열을 요소로 갖는 튜플을 반환하는 함수
function countAndDescribe<T extends Length>(element: T): [T, string] {
  let description = "got no value";
  if (element.length === 1) {
    description = `got ${element.length} element.`;
  } else if (element.length > 1) {
    description = `got ${element.length} elements.`;
  }

  return [element, description];
}

console.log(countAndDescribe("hi")); //  ["hi", "got 2 elements."]
console.log(countAndDescribe(["hello", "world"])); //  [["hello", "world"], "got 2 elements."]

console.log(countAndDescribe(10)); //  Argument of type 'number' is not assignable to parameter of type 'Length'.
```

`countAndDescribe()` 함수의 파라미터인 `element`의 T 타입은 구체적으로`Length` 타입이 지정되어 있다.

따라서 `Length` 타입에 지정된 `length` 속성은 반드시 가지고 있어야 한다.

- length 속성이 있는 **string**, **array** 타입은 파라미터로 들어올 수 있고,
- length 속성이 없는 **number** 타입은 파라미터로 들어가면 오류가 발생한다.

### 🍇 keyof

```ts
function extractAndConvert(obj: object, key: string) {
  return `value of Obj : ${obj[key]}`;
  // Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{}'.
  // No index signature with a parameter of type 'string' was found on type '{}'.
}
```

객체와 키를 받아서 값을 호출하려는 함수에 `object`와 `string` 타입만 전달해주면 에러가난다.

`obj` 의 타입인 `{}` 안에 어떤 타입의 속성이 들어갈 지 정해주지 않았기 때문에 타입스크립트는 `{}` 안에 `string` 타입의 키를 찾을 수 없기 때문이다.

```ts
function extractAndConvert<T extends object, U extends keyof T>(
  obj: T,
  key: U
) {
  return obj[key];
}

extractAndConvert({ name: "max" }, "name");
extractAndConvert({ age: 20 }, "name"); // Argument of type '"name"' is not assignable to parameter of type '"age"'.
```

이를 제네릭 타입으로 고쳐줄 수 있다.
이 때, 객체의 키로 사용되는 파라미터의 타입에 `extends keyof 객체 타입`을 지정해주면, 해당 타입의 객체의 키로 사용된다는 것을 명시해줄 수 있다.

객체에 해당 키가 없다면 타입스크립트에서 오류를 알려주기 때문에,
잘못된 키로 객체 값을 참조하는 실수를 방지할 수 있다.

> ### 제네릭이 아닌 keyof 와 차이점
>
> ```ts
> function get<T>(obj: T, key: keyof T) {
>   return obj[key];
> }
>
> const roles = { favorite: "tiger", rank: 1 };
>
> /* value의 타입은 string | number 가 된다.*/
> let value = get(roles, "favorite");
> value.toUppercase(); // Property 'toUppercase' does not exist on type 'string | number'.
> // Property 'toUppercase' does not exist on type 'string'.
> ```
>
> 반환되는 value는 roles가 가진 모든 value값들의 유니언타입이 된다.

## 🧞‍♀️ 제네릭 인터페이스

```ts
interface Box<T> {
  genericKey: T; // Box 타입을 갖는 객체의 genericKey는 인스턴스에서 지정하는 타입만 가능하다.
}

let strBox: Box<string> = {
  genericKey: "문자만 가능",
};

let numberBox: Box<number> = {
  genericKey: "숫자만 가능", // Type 'string' is not assignable to type 'number'.
};
```

## 🧞‍♀️ 제네릭 클래스

클래스에도 제네릭을 사용할 수 있다.

```ts
class DataStorage<T> {
  private data: T[] = [];

  addItem(item: T) {
    this.data.push(item);
  }

  removeItem(item: T) {
    if (this.data.indexOf(item) === -1) return; // item 이 data 안에 없으면 아무 동작 안함
    this.data.splice(this.data.indexOf(item), 1);
  }

  getItems() {
    return [...this.data];
  }
}

// 'string' 타입만 요소로 추가할 수 있는 인스턴스 객체
const textStorage = new DataStorage<string>();

// 'number' 타입은 오류가 난다.
textStorage.addItem(1); // Argument of type 'number' is not assignable to parameter of type 'string'.

textStorage.addItem("May");
textStorage.addItem("Janny");

textStorage.removeItem("May");

console.log(textStorage.getItems()); // ['Janny']
```

이때 원시타입이 아닌, 참조타입은 예상대로 동작하지 않을 수 있다.

```ts
class DataStorage<T> {
  // ... 생략
}

const objStorage = new DataStorage<object>();
objStorage.addItem({ name: "Mark" });

const KateObj = { name: "Kate" };
objStorage.addItem(KateObj);

// 참조값이 다르므로 삭제되지 않는다.
objStorage.removeItem({ name: "Kate" });
console.log(objStorage.getItems());
/* [{name: 'Mark'}, {name: 'Kate'}] */

// 참조값이 같으므로 삭제된다.
objStorage.removeItem(KateObj);
console.log(objStorage.getItems());
/* [{name: 'Mark'}] */
```

객체는 참조값의 주소를 저장하기 때문에,
`addItem()` 과 `removeItem()` 에 전달된 `{name: 'Kate'}` 객체는 같아 보여도 서로 다른 주소를 가진 다른 객체이다.

따라서 객체의 주소를 저장한 변수를 전달하는 것과 같이, 동일한 주소를 전달해야 제대로 동작한다.

```ts
class DataStorage<T extends string | number | boolean> {
  // ... 내용 생략
}

const objStorage = new DataStorage<object>(); // Type 'object' does not satisfy the constraint 'string | number | boolean'.
```

제거해야할 때마다 정확학 객체의 주소를 알아내는 것은 번거로우므로,
`extends` 키워드로 `<T extends string | number | boolean>` 처럼 클래스를 원시타입의 인스턴스만 만들도록 지정할 수 있다.

### 🍇 클레스의 제네릭 메서드

이 외에도 클래스 전체가 아닌 특정 메서드만 제네릭으로 만드는 것도 가능하다.

```ts
class Obj<Key> {
  key: Key;
  constructor(key: Key) {
    this.key = key;
  }

  createProperty<Value>(value: Value) {
    // 메서드는 다른 타입의 제네릭으로 생성
    return { key: this.key, value };
  }
}

let newObj = new Obj("ages");
let output = newObj.createProperty(100);
/*  Obj<string>.createProperty<number>(value: number): {
    key: string;
    value: number;
}
*/
console.log(output); // { "key": "ages", "value": 100 }
```

### 🍇 static 메서드 제네릭

`static 메서드` : 프로토타입 체인이 아닌, class에만 있는 메서드
인스턴스 객체에서는 호출할 수 없고, class의 메서드로만 호출할 수 있다.

```ts
class Book<B> {
  instanceMethod(input: B) {
    return input;
  }

  // 자체 타입만 지정 가능하고, 클래스에 선언된 다른 타입에 접근할 수 없다.
  static staticMethod<C>(input: C) {
    let incorrect: B; // Static members cannot reference class type parameters.

    return input;
  }
}
```

클래스의 정적 메서드(static method)는 자체 타입 매개변수를 선언할 수 있지만,\
클래스에 선언된 어떤 타입 매개변수에도 접근할 수 없다.

## 🧞‍♀️ Generic 타입과 Union 타입의 차이

```ts
class Ary {
  element: string | number;
  constructor(element: string | number) {
    this.element = element;
  }

  createAry(input: string | number) {
    // 메서드는 다른 타입의 제네릭으로 생성
    return [this.element, input];
  }
}

// 파라미터로 string 또는 number 유니언타입 전부가 들어올 수 있다.
// 제네릭과 다르게 인스턴스를 하나의 타입으로 고정할 수 없다.
let unionAry = new Ary("apple");

/* string 타입만 넣어줬지만, 매개변수로 number 타입도 들어올 수 있기 때문에, string 메서드를 쓸 수 없다. */
unionAry.createAry("banana")[0].toUppercase();
// Property 'toUppercase' does not exist on type 'string | number'.
// Property 'toUppercase' does not exist on type 'string'.
```

유니언타입은 함수를 호출할 때마다 여러 타입들 중 하나로 호출할 수 있는 함수가 필요한 경우와 모든 메서드 호출이나 모든 함수 호출마다 다른 타입을 지정하고자 할 때 유용하다.

제네릭 타입은 여러 타입들 중 특정 타입을 고정하거나 전체 클래스 인스턴스에 걸쳐 같은 함수를 사용하거나 전체 함수에 걸쳐 같은 타입을 사용하려고 할 때 유용하다.

## 🧞‍♀️ 제네릭 타입 별칭

제네릭 타입 별칭은 일반적으로 제네릭 함수 타입을 설명하는 함수와 함께 사용된다.

```ts
type FuncType<Input, Output> = (input: Input) => Output;

let getLength: FuncType<string, number>; // Input에 string, Output 에 number 타입이 지정됐다.
getLength = (input) => input.length;

getLength = (text) => text.toUpperCase(); // Type 'string' is not assignable to type 'number'.
```

### 🍇 제네릭 판별된 유니언

```ts
type Result<Data> = Failure | Success<Data>;

interface Failure {
  error: Error;
  succeeded: false;
}

interface Success<Data> {
  data: Data;
  succeeded: true;
}

function printResult(result: Result<string>) {
  if (result.succeeded) {
    // 유니언 타입 중 Success 타입인 경우
    return `Good! data is ${result.data}`;
    // Result<string> 이므로 result.data는 string 타입이다.
  } else {
    // 유니언 타입 중 Failure 타입인 경우
    return `check ${result.error.message}`;
  }
}
```

## 🧞‍♀️ 타입스크립트의 내장 utility 타입

### 🍇 Partial 타입

`CourseGoal` 타입을 갖는 객체를 리턴하는 함수를 아래와 같이 만들 수 있다.

```ts
interface CourseGoal {
  title: string;
  description: string;
  completeUntil: Date;
}

function createCourseGoal(
  title: string,
  description: string,
  date: Date
): CourseGoal {
  return { title, description, completeUntil: date };
}
```

그런데 이때 만약 객체를 바로 리턴하는 것이 아니라, 함수 바디 내에서 객체를 생성해서 리턴해야한다면 오류가 발생한다.

```ts
interface CourseGoal {
  title: string;
  description: string;
  completeUntil: Date;
}

function createCourseGoal(
  title: string,
  description: string,
  date: Date
): CourseGoal {
  let courseGoal = {};
  courseGoal.title = title; // `{}` 형식에 `title` 속성이 없습니다.
  courseGoal.description = description; // `{}` 형식에 `description` 속성이 없습니다.
  courseGoal.completeUntil = date; // `{}` 형식에 `completeUntil` 속성이 없습니다.

  return courseGoal; // `{}` 형식에 `CourseGoal` 형식의 `title`, `description`, `completeUntil` 속성들이 없습니다.
}
```

새로 생성하는 객체에 `:CourseGoal` 타입을 지정해주면 오류를 해결할 수 있다.

```ts
function createCourseGoal(): CourseGoal {
  // ... 생략
  let courseGoal: CourseGoal = {}; // `{}` 형식에  `CourseGoal` 형식의 `title`, `description`, `completeUntil` 속성들이 없습니다.
  courseGoal.title = title;
  courseGoal.description = description;
  courseGoal.completeUntil = date;

  return courseGoal;
}
```

하지만 초깃값인 `={}` 빈 객체에 `CourseGoal` 타입 속성이 없으므로 오류가 발생하는데,

이때 **`Partial`** 타입을 사용할 수 있다.

```ts
function createCourseGoal(): CourseGoal {
  // ... 생략
  let courseGoal: Partial<CourseGoal> = {}; // `{}` 형식에  `CourseGoal` 형식의 `title`, `description`, `completeUntil` 속성들이 없습니다.
  courseGoal.title = title;
  courseGoal.description = description;
  courseGoal.completeUntil = date;

  return courseGoal as CurseGoal;
}
```

`Partial` 타입 키워드는 타입스크립트에게 해당 변수가 뒤에 오는 제네릭 타입이 될 값이라는 것을 알려준다.

즉, 뒤에 오는 제네릭 타입의 모든 속성을 **선택적(optional) 속성**이 되도록 만들어준다.

- 한편 `return 값인 courseGoal`도 `CourseGoal` 타입의 모든 속성들을 optional 하게 가지므로,
  return 값을 `as CurseGoal`로 정확하게 타입을 지정해주어야 오류가 나지 않는다.

### 🍇 Readonly

```ts
const names: Readonly<string[]> = ["Max", "Anna"];
names.push("Manu"); // Property 'push' does not exist on type 'readonly string[]'.
names.pop(); // Property 'pop' does not exist on type 'readonly string[]'.
```

해당 값을 읽기 전용으로 만들어준다.

### 🍇 [또 다른 Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

---

참고 : [udemy 【한글자막】 Typescript :기초부터 실전형 프로젝트까지 with React + NodeJS
](https://www.udemy.com/course/best-typescript-21/learn/lecture/27937846?start=75#content)
