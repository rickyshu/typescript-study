## 🐳 클래스

### 🌊 자바스크립트로 컴파일

class를 자바스크립트로 컴파일 할 때 컴파일옵션을 최신 버전에서 es5 이하로 낮추면 class 대신 function으로 컴파일링 된다.\
낮은 버전으로 바꾸는 법
`tsconfig.json` 파일에서 `"compilerOptions"`의 `"target": "es5"`로 바꾼다.\
참고 : [Typescript 컴파일시 세부설정 (tsconfig.json)](https://codingapple.com/unit/typescript-tsconfig-json/)\
TypeScript Playground 에서 TS Config 에서 Target을 ES5 이하로 바꾼다.

```ts
// TypeScript

class Book {
  readonly title: string;

  constructor(_title: string, readonly series: string) {
    this.title = _title;
    this.series = series;
  }
}

let harryPotter = new Book("Harry Potter", "마법사의 돌");
console.log(harryPotter.series);
```

```js
// JavaScript ES2017

"use strict";
class Book {
  constructor(_title, series) {
    this.series = series;
    this.title = _title;
    this.series = series;
  }
}
let harryPotter = new Book("Harry Potter", "마법사의 돌");
console.log(harryPotter.series);
```

```js
// JavaScript ES5

"use strict";
var Book = /** @class */ (function () {
  function Book(_title, series) {
    this.series = series;
    this.title = _title;
    this.series = series;
  }
  return Book;
})();
var harryPotter = new Book("Harry Potter", "마법사의 돌");
console.log(harryPotter.series);
```

### 🌊 클래스 속성과 메서드

클래스의 속성을 읽거나 쓰려면 명시적으로 선언해야한다.\
클래스 생성자는 매개변수와 관련하여 전형적인 클래스 메서드처럼 취급된다.

```ts
class Greeting {
  message: string; /* 클래스의 속성을 읽거나 쓰려면 명시적으로 선언해야한다.*/

  constructor(message: string) {
    /* 생성자 함수 */
    this.message = message;
    console.log(`${message}!`);

    this.greet = greet; // Property 'greet' does not exist on type 'Greeting'.
  }

  withName(name: string) {
    /* 메서드 */
    console.log(`${this.message}, ${name}!`);
  }
}

new Greeting("Hello!"); // "Hello!"
new Greeting("Hi").withName("Mina");
// "Hi!"
// "Hi, Mina!"

/* 선언된 파라미터의 수와 아규먼트의 수가 같아야 한다. */
new Greeting(); // Expected 1 arguments, but got 0.
new Greeting("Hi").withName(); // Expected 1 arguments, but got 0.
```

### 🌊 함수 속성

```js
class Greeting {
  sayHello() {} // 메서드 체인으로 연결되는 프로토타입에 정의된 메서드
  sayHi = () => {}; // 각 인스턴스 객체마다 새롭게 할당되는 메서드
}

console.log(new Greeting().sayHello === new Greeting().sayHello); // true
console.log(new Greeting().sayHi === new Greeting().sayHi); // false
```

### 🌊 엄격한 초기화 검사

```ts
class Book {
  /* 타입을 지정하지 않았지만 초깃값이 있으므로 number 타입로 자동 정의됨 */
  pages = 100;
  title: string;

  /* 사용되지 않는 값에는 undefined 타입이 있어야 한다.  */
  author: string | undefined;
  movie: string; //Property 'movie' has no initializer and is not definitely assigned in the constructor.

  constructor(t: string) {
    this.title = t;
  }
}

/* 자바스크립트 파일로 컴파일은 된다. */
let HarryPotter = new Book("HarryPotter");
console.log(HarryPotter.movie); // undefined
console.log(HarryPotter.pages); // 100
```

```ts
class Book {
  /* !를 안붙이면 오류 발생
   characters: string[] // Property 'characters' has no initializer and is not definitely assigned in the constructor. */
  characters!: string[];

  aliveCharacters(characters: string[]) {
    this.characters = characters;
  }

  deadCharacter() {
    return this.characters.pop();
  }
}

const harryPotter = new Book();
harryPotter.aliveCharacters(["해리", "헤르미온느", "론", "도비"]);
let characterName = harryPotter.deadCharacter();
console.log(characterName); // 도비
```

`!`를 붙이면 타입스크립트에 속성이 처음 사용되기 전에 undefined 값이 할당된다.

- constructor() 없이 프로퍼티 사용할 수 있음

### 🌊 선택적 속성

```ts
class Book {
  title: string;
  author?: string;

  constructor(_title: string) {
    this.title = _title;
  }
}
```

`?`를 추가하면 속성을 옵션으로 선언한다. 즉 클래스 생성자에서 할당하지 않아도 된다.

- `:undefined`를 포함하는 유니언 타입과 거의 동일하다.

### 🌊 읽기 전용 속성

```ts
class Book {
  readonly title: string;
  author?: string;

  constructor(_title: string) {
    this.title = _title;
  }

  emphasize() {
    this.title += "⭐️"; // Cannot assign to 'title' because it is a read-only property.
  }
}

let harryPotter = new Book("Harry Potter");
harryPotter.title += "and the Sorcerer's Stone"; // Cannot assign to 'title' because it is a read-only property.
```

선언된 속성 이름 앞에 readonly 키워드를 추가하면 읽기 전용 속성이 된다.

- `readyonly` 속성은 선언된 위치 또는 생성자에서 초깃값만 할당할 수 있다.
- 클래스 내 메서드 또는 클래스 외부 등 모든 위치에서 읽을 수만 있고 값을 변경할 수 없다.

> npm 패키지로 게시한 코드를 사용하는 외부인이 readonly 제한자를 지키지 않을 수 있으므로, (특히 자바스크립트를 사용하며, 타입 검사를 하지 않는 경우)\
> 반드시 읽기 전용 보호가 필요하다면 **# private** 필드나 **get()** 게터를 사용하는 것이 권장된다.

```ts
class Book {
  readonly title: string =
    "해리포터"; /* string 타입의 다른 값으로 할당될 수 있다. */
  readonly publisher =
    "문학수첩"; /* "문학수첩" 외 다른 타입 값으로 할당될 수 없다. */

  constructor(_title: string, _publisher: string) {
    this.title = _title;
    this.publisher = _publisher; // Type 'string' is not assignable to type '"문학수첩"'.
  }
}
```

readonly 로 타입을 지정하면 constructor 내부에서 같은 타입의 다른 값으로 할당할 수 있다.
readonly 로 타입을 지정하지 않고, 선언된 자리에서 값을 할당하면, 해당 값이 타입이 되므로 constructor 내부에서도 그 외 다른 값으로 할당할 수 없다.

```ts
class Book {
  readonly title: string;
  /* series의 타입을 미리 지정해주지 않아도 오류가 나지 않는다. */

  constructor(_title: string, readonly series: string) {
    this.title = _title;
    this.series = series;
  }
}

let harryPotter = new Book("Harry Potter", "마법사의 돌");
console.log(harryPotter.series);
```

생성자 함수의 파라미터와 함수 바디에서의 이름이 같을 때,
파라미터에 `readonly`를 지정하면, 프로퍼티 타입 애너테이션을 생략할 수 있다.

- 파라미터에 readonly 지정되어 있는 프로퍼티는 자바스크립트로 컴파일링될 때, 생성자 함수 바디에서 2번 선언된다.

### 🌊 타입으로서의 클래스

타입 시스템에서의 클래스는 클래스 자체와 타입 에너테이션에서 사용할 수 있는 타입 모두를 생성한다.

```ts
class Book {
  getPages() {
    return [100];
  }
}

/* 타입으로써의 Book */
function getBookPages(book: Book) {
  console.log(book.getPages());
}

/* class로써의 Book, 인스턴스 객체 생성 */
getBookPages(new Book());
/* 인스턴스 객체와 똑같은 구조이므로 가능 */
getBookPages({ getPages: () => [500, 100, 200] });

/* 반환 타입이 number[] 로 똑같아야 한다. */
getBookPages({ getPages: () => ["100페이지"] }); // Type 'string' is not assignable to type 'number'.
```

### 🌊 클래스와 인터페이스

클래스 이름 뒤에 `implements` 와 인터페이스 이름을 추가해서 클래스의 해당 인스턴스가 인터페이스를 준수한다고 선언한다.

```ts
interface Book {
  title: string;
  getPages(title: string): number;
  series?: string;
}

/* 선택적 속성("?") 외에는 인터페이스의 모든 속성이 정의된 타입으로 클래스에도 있어야 한다. */
class Books implements Book {
  title: string;
  movie?: string; /* 인터페이스에 없는 속성을 추가로 더 하는 것은 괜찮다.*/

  constructor(_title: string, _movie?: string) {
    this.title = _title;
    this.movie = _movie;
  }

  getPages(title: string) {
    return 400;
  }
}
```

```ts
interface Person {
  name: string;
  phone: number;
}

interface Friend {
  school: string;
}

/* ,쉼표로 여러 인터페이스를 사용할 수 있다. */
class MyFriend implements Person, Friend {
  name: string;
  school: string;
  phone: number;

  constructor(_name: string, _school: string, _phone: number) {
    this.name = _name;
    this.school = _school;
    this.phone = _phone;
  }
}

/* 같은 이름의 다른 타입의 속성을 가진 인터페이스들을 같이 쓸 수 없다. */
interface Contact {
  phone: string;
}

class Wrong implements Person, Contact {
  name: string;
  phone: number; // Property 'phone' in type 'Wrong' is not assignable to the same property in base type 'Contact'.
  // Type 'number' is not assignable to type 'string'.

  constructor(_name: string, _phone: number) {
    this.name = _name;
    this.phone = _phone;
  }
}
```

### 🌊 클래스 확장

`extends` 키워드로 확장한다.
`super` 키워드로 부모의 메서드나 프로퍼티를 복사해온다.

```ts
class Person {
  name: string;
  constructor(_name: string) {
    this.name = _name;
  }
}

class Student extends Person {
  school: string;
  constructor(_name: string, _school: string) {
    super(_name); // super는 항상 this보다 먼저 호출되어야 한다.
    this.school = _school;
  }
}
/* Student 클래스에는 Person 타입에서 필요로 하는 모든 속성이 있으므로 new Student를 할 수 있다. */
let Jake: Person;
Jake = new Person("Jake");
Jake = new Student("Jake", "SKY university");

let Katy: Student;
Katy = new Student("Katy", "Han school");

/* Person 클래스에는 타입으로 지정된 Student에 있는 속성이 없기 때문에 오류가 난다. 
만약 school?: string 로 선택적 속성이었다면 오류가 나지 않는다. */
Katy = new Person("Katy"); // Property 'school' is missing in type 'Person' but required in type 'Student'.
```

기본 클래스에 하위 클래스가 가지고 있는 모든가 없으면 더 구체적인 하위 클래스가 필요할 때 사용할 수 없다.

```ts
class Person {
  greeting(message: string) {
    return `${message}`;
  }
}

class Student extends Person {
  message?: string;

  greeting(_message: string) {
    this.message = super.greeting(_message); // super. 메서드이름으로 상위 클래스의 메서드를 가져올 수 있다.
    return super.greeting(`message: ${_message}`); // return 타입이 같은 string 타입이어서 OK
  }
}

let student = new Student();

console.log(student.message); // undefined
console.log(student.greeting("hi")); //  "message: hi"
console.log(student.message); // "hi"

/* 자식 클래스의 메서드의 타입(파라미터 또는 리턴값의 타입)이 다르면 오류가 난다. */
class Wrong extends Person {
  greeting(_message: string) {
    return super.greeting(_message).length;
  }
}
// Property 'greeting' in type 'Wrong' is not assignable to the same property in base type 'Person'.
//  Type '(_message: string) => number' is not assignable to type '(message: string) => string'.
//    Type 'number' is not assignable to type 'string'.
```

하위 클래스의 메서드의 타입은 기본(super)클래스의 타입과 같아야 한다.

```ts
class Animal {
  eat?: string; // undefined 또는 string 타입
}

class Cat extends Animal {
  eat: string; // string 타입으로 좁히는 것 OK
  constructor(_eat: string) {
    super();
    this.eat = _eat;
  }
}

class Dog extends Animal {
  eat: undefined | string | boolean; // 더 많은 타입으로 확장하는 것은 오류가 발생한다.
  constructor(_eat: string) {
    super();
    this.eat = _eat;
  }
}
// Property 'eat' in type 'Dog' is not assignable to the same property in base type 'Animal'.
//  Type 'string | boolean | undefined' is not assignable to type 'string | undefined'.
//    Type 'boolean' is not assignable to type 'string'.
```

### 🌊 추상 클래스

추상화 하려는 **클래스 이름**과 **메서드** 앞에 타입스크립트의 `abstract` 키워드 추가

- 기본(super)클래스에서의 메서드 구현을 건너뛰고, 하위 클래스에서 해당 메서드를 정의한다.
  - 기본(super) 클래스에서는 메서드의 타입만 지정한다.(인터페이스와 동일한 방식)
  - 각 하위 클래스마다 메서드를 다르게 정의할 수 있다.
- 추상 클래스로 인스턴스 객체를 만들 수 없다.
- 타입 애너테이션으로 사용할 수 있다.

추상 클래스에 abstract 키워드를 안 붙인 일반적인 구현 메서드도 포함할 수 있다.

```ts
// 기본(super) 클래스 = 추상클래스
abstract class School {
  readonly name: string;

  constructor(_name: string) {
    this.name = _name;
  }

  abstract getName(): string[];
}

// 하위 클래스
class MiddleSchool extends School {
  getName() {
    return [this.name];
  }
}

let SKY_School = new MiddleSchool("SKY");
console.log(SKY_School.getName()); // ["SKY"]

// 하위 클래스
class HighSchool extends School {
  getName() {
    return ["한국 고등학교", "최고"];
  }
}

let 한국고 = new HighSchool("한국");
console.log(한국고.getName()); // ["한국 고등학교", "최고"]

/* 추상 클래스를 직접 인스턴스화 할 수 없다. */
let wrongClass = new School("알 수 없는 학교"); // Cannot create an instance of an abstract class.
```

> **추상 클래스와 인터페이스**\
> 관련 클래스 집합에 대한 공통 기반을 제공하려는 경우 추상 클래스를 사용하고,\
> 클래스 또는 개체가 반드시 수정해야하는 속성 또는 메서드의 공통 집합을 정의하려는 경우 인터페이스를 사용한다.

### 🌊 멤버 접근성

> 접근성 키워드는 타입스크립트에만 존재하며, 자바스크립트로 컴파일시 삭제된다.\
> public(기본값) : 모든 곳에서 누구나 접근 가능\
> protected : 클래스 내부 또는 하위 클래스에서만 접근 가능\
> private : 클래스 내부에서만 접근 가능\
> readonly : 읽기 전용\
> static : 정적 프로퍼티, 정적 메서드 (인스턴스 객체에서는 접근 불가, 클래스에서만 호출 가능)

> 자바스크립트의 접근성 키워드\
> static : 동일\
> `#` : private 필드, 클래스 내부에서만 접근 가능

```ts
class Base {
  #truePrivate = 4; /* Base 내부에서만 사용 가능 */
  private isPrivate = 3; /* Base 내부에서만 사용 가능 */
  protected isProtected = 2;
  public isPublicField = 1;
  isPublic = 0 + this.#truePrivate + this.isPrivate;

  func() {
    let value = this.#truePrivate + this.isPrivate;
    return value;
  }
}

class SubClass extends Base {
  example() {
    this.isPublic;
    this.isPublicField;
    this.isProtected; /* 하위 클래스까지만 접근 가능*/

    this.isPrivate; // Property 'isPrivate' is private and only accessible within class 'Base'.
    this.#truePrivate; // Property '#truePrivate' is not accessible outside class 'Base' because it has a private identifier.
  }
}

new SubClass().isPublic;
new SubClass().isPublicField;

new SubClass().isPrivate; // Property 'isPrivate' is private and only accessible within class 'Base'.
new SubClass().isPrivate; // Property 'isPrivate' is private and only accessible within class 'Base'.
new SubClass().#truePrivate; // Property '#truePrivate' is not accessible outside class 'Base' because it has a private identifier.
```

```ts
class AccessClass {
  private readonly className = "can't access";
  public static readonly value = "value";

  static myMethod() {
    console.log("AccessClass의 메서드");
  }
}

console.log(AccessClass.value); // : "value"
AccessClass.myMethod(); //  //  "AccessClass의 메서드"

AccessClass.className; // Property 'className' does not exist on type 'typeof AccessClass'.
AccessClass.value = "change?"; // Cannot assign to 'value' because it is a read-only property.
new AccessClass.myMethod(); // 'new' expression, whose target lacks a construct signature, implicitly has an 'any' type.
```

접근성 키워드 + static + readonly 순서로 적어야 한다.\
접근성 키워드, static, readonly 각각 단독으로 사용할 수도 있다.
