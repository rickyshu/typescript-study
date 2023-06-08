> 러닝타입스크립트 책에서 가져온 내용은 ""를 붙임

## Chapter 8. 클래스

### 클래스 간단하게 다시 보기

- constructor
- prototype method
- static method
- instance property
- instance method
- class Field : public, private, static

<br>

### 8.1 클래스 메서드

- "타입스크립트는 독립 함수를 이해하는 것과 동일한 방식으로 메서드를 이해합니다."
  - 일반적인 함수와 동일한 방식을 작동
- "`constructor`는 매개변수와 관련하여 전형적인 클래스 메서드처럼 취급됩니다."
  - 클래스 메서드는 독립 함수와 동일한 방식으로 작동하기에 일반 함수와 동일하다.

<br>

### 8.2 클래스 속성

- "클래스 속성을 읽거나 쓰려면 명시적으로 선언해야 합니다."
- "인터페이스와 동일한 구문을 사용해 선언"
- "선택적으로 타입 애너테이션이 붙습니다." 안붙이면 any
- "클래스 속성을 명시적으로 선언하면 타이"

```ts
class FiledTrip {
  destination
  name1

  constructor(destination: string) {
    this.destination = destination
    this.arrival = '도착'
  }
}

const trip = new FiledTrip('string')
trip.destination
trip.arrival
```

<br>

### 8.2.1 함수 속성

- "값이 함수인 속성을 선언하는 방식도 있습니다."

```ts
class FiledTrip {
  // 프로퍼티에 함수를 선언
  propertyFunc: () => void

  constructor() {
    this.propertyFunc = () => console.log(1)
  }

  // 메서드
  method() {
    console.log(1)
  }
}

const trip1 = new FiledTrip()
const trip2 = new FiledTrip()

console.log(trip1.propertyFunc === trip2.propertyFunc) // false
console.log(trip1.method === trip2.method) // true
```

<br>

### 8.2.2 초기화 검사

- 엄격한 컴파일러 설정이 활성화된 상태에서
  - 속성을 명시적으로 선언한 경우, constructor로 초기화시 설정하지 않으면 오류
- strictPropertyInitialization
  - Check for class properties that are declared but not set in the constructor.

### 확실하게 할당된 속성 `!`

- 의도적으로 클래스 속성을 할당하지 않는 경우
- 속성 뒤에 `!` 추가
- 검사를 비활성화, `undefined` 할당

```ts
class ActivitiesQueue {
  pending!: string[]

  initialize(pending: string[]) {
    this.pending = pending
  }

  next() {
    return this.pending.pop()
  }
}
const activities = new ActivitiesQueue()
activities.initialize(['eat', 'sleep', 'learn'])
activities.next()
```

```ts
// 개선 코드
class ActivitiesQueue {
  private _pending: string[]

  constructor(pending: string[] = []) {
    this._pending = pending
  }

  get pending() {
    return this._pending
  }

  initialize(pending: string[]) {
    this._pending = pending
  }

  next() {
    if (this._pending.length === 0) {
      throw new Error('Queue is empty')
    }
    return this._pending.pop()
  }
}

const activities = new ActivitiesQueue()

queue._pending // Error : Property '_pending' is private and only accessible within class 'ActivitiesQueue'.

activities.initialize(['eat', 'sleep', 'learn'])
activities.next()
activities.next()
activities.next()
activities.next() // Runtime Error : Queue is empty
```

<br>

### 8.2.3 선택적 속성 `?`

- "`| undefined`를 포함하는 유니언 타입과 거의 동일하게 작동합니다."
- 초기화 검사에서 문제가 되지 않음

```ts
class Person {
  name: string
  age?: number

  constructor(name: string, age?: number) {
    this.name = name
    this.age = age
  }
}

const person1 = new Person('Alice', 30)
const person2 = new Person('Bob')

person2.age
person2.age * 2 // Error: 'person2.age' is possibly 'undefined'
```

<br>

### 8.2.4 읽기 전용 속성 `readonly`

- 속성을 읽기 전용으로 선언
- "`readonly`로 선언된 속성은 선언된 위치 또는 생성자에서 초깃값만 할당할 수 있습니다."
- "클래스 내의 메서드를 포함한 다른 모든 위치에서 속성은 읽을 수 만 있고, 쓸 수는 없습니다."

```ts
class Person {
  readonly name: string
  readonly age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}`
  }
}

const person1 = new Person('John', 25)
console.log(person1.getDetails())

person1.name = 'Jane' // Error: Cannot assign to 'name' because it is a read-only property.
```

```ts
class RandomQuote {
  readonly explicit: string = 'Home is the nicest word there is.' // 타입 확장
  readonly implicit = 'Home is the nicest word there is.'
  constructor() {
    if (Math.random() > 0.5) {
      this.explicit = "We start learning the minute we're born. 111" // OK
      this.explicit = "We start learning the minute we're born. 222" // OK
      this.implicit = "We start learning the minute we're born." // Error
    }
  }
}
```

<br>

### 8.3 타입으로서의 클래스

- "타입 시스템에서의 클래스는 클래스 선언이 런타임 값(클래스 자체)과 타입 애너테이션에서 사용할 수 있는 타입을 모두 생성한다는 점에서 상대적으로 독특합니다."

<br>

### 8.4 클래스와 인터페이스

- "클래스 이름 뒤에 implements 키워드와 인터페이스 이름을 추가함으로써 클래스의 해당 인스턴스가 인터페이스를 준수한다고 선언할 수 있습니다."

```ts
interface Person {
  name: string
  age: number
  greet(): void
}

class Student implements Person {
  name: string
  age: number
  studentId: string

  constructor(name: string, age: number, studentId: string) {
    this.name = name
    this.age = age
    this.studentId = studentId
  }

  greet() {
    console.log(`My name is ${this.name}, and My Id is ${this.studentId}.`)
  }

  study() {
    console.log(`I'm studying`)
  }
}

const student = new Student('Alice', 20, '12345')
student.greet()
student.study() // 클래스가 추가 멤버를 갖는 것을 제한하지는 않는다.
// Student 클래스가 Person 인터페이스에 정의된 모든 멤버에 대한 구현을 제공하는 한 TypeScript에서 완벽하게 허용됩니다.
// study 메서드는 Student 클래스에 고유한 추가 기능을 제공하는 데 사용할 수 있습니다.

// 객체의 경우
const john: Person = {
  name: 'john',
  age: 20,
  studentId: 23456, // Error: ...is not assignable to type 'Person'.
  greet: function () {
    console.log(`My name is ${this.name}, and My Id is ${this.studentId}.`)
  },
}
```

<br>

### 8.4.1 다중 인터페이스 구현

- 개수 제한 없이 인터페이스를 사용할 수 있다.

```ts
interface Person {
  name: string
  age: number
}

interface Student {
  studentId: string
  study(): void
}

class CollegeStudent implements Person, Student {
  name: string
  age: number
  studentId: string

  constructor(name: string, age: number, studentId: string) {
    this.name = name
    this.age = age
    this.studentId = studentId
  }

  study() {
    console.log(`I'm studying`)
  }
}

const collegeStudent = new CollegeStudent('Alice', 20, '12345')
console.log(collegeStudent.name)
console.log(collegeStudent.studentId)
collegeStudent.study()
```

<br>

### 8.5 클래스 확장 `extends`

- 자바스크립트 개념에 타입 검사를 추가합니다.
- 수퍼 - 서브 클래스

```ts
class Person {
  name: string

  constructor(name: string) {
    this.name = name
  }

  greet() {
    console.log(`Hi, my name is ${this.name}.`)
  }
}

class Employee extends Person {
  title: string

  constructor(name: string, title: string) {
    super(name)
    this.title = title
  }

  promote(newTitle: string) {
    console.log(`Congratulations, ${this.name}! You've been promoted to ${newTitle}.`)
    this.title = newTitle
  }
}

const employee = new Employee('Alice', 'Manager')
employee.greet()
employee.promote('Senior Manager')
```

### 8.5.1 할당 가능성 확장

### 8.5.2 재정의된 생산자 - constructor, super

### 8.5.3 재정의된 메서드 - super

### 8.5.4 재정의된 속성

- 속성도 오버라이드가능
- 허용된 타입 내에서 가능

- "서브클래스에서 동일한 이름으로 새 메서드를 다시 선언할 수 있습니다."
- "새 메서드의 타입도 수퍼클래스의 메서드 대신 사용할 수 있어야 한다는 점을 명심하세요."
- Override

<br>

### 추상 클래스 `abstract`

- 직접 인스턴스화할 수 없다.
- 인터페이스와 동일한 방식

```ts
abstract class Shape {
  static className = 'abstract class shape'
  abstract area(): number
}

class Rectangle extends Shape {
  private width: number
  private height: number

  constructor(width: number, height: number) {
    super()
    this.width = width
    this.height = height
  }

  area(): number {
    console.log(Shape.className)
    return this.width * this.height
  }
}

class Circle extends Shape {
  private radius: number

  constructor(radius: number) {
    super()
    this.radius = radius
  }

  area(): number {
    return Math.PI * this.radius * this.radius
  }
}

const rectangle = new Rectangle(5, 10)
const circle = new Circle(5)
console.log(Shape.className)
//const shape = new Shape() // Error : Cannot create an instance of an abstract class.
```

#### 그렇다면 인터페이스와 차이점은?

- 추상 클래스는 추상 메서드와 구현 메서드를 모두 포함할 수 있는 반면 인터페이스는 메서드 시그니처만 포함할 수 있습니다.
- 추상 클래스는 멤버에 대해 공개, 보호 또는 비공개 액세스 수정자를 가질 수 있는 반면 인터페이스는 해당 멤버에 대해 공개 액세스 수정자만 가질 수 있습니다.
- 일반적으로 관련 클래스 집합에 대한 공통 기반을 제공하려는 경우 추상 클래스를 사용하고 클래스 또는 개체가 반드시 수행해야 하는 속성 및/또는 메서드의 공통 집합을 정의하려는 경우 인터페이스를 사용합니다.

<br>

```ts
{
  type CoffeeCup = {
    shots: number
    hasMilk?: boolean
    hasSugar?: boolean
  }

  // interface
  interface CoffeeMaker {
    makeCoffee(shots: number): CoffeeCup
  }

  // abstract
  abstract class CoffeeMachine implements CoffeeMaker {
    private static BEANS_GRAMM_PER_SHOT: number = 7
    private coffeeBeans: number = 0

    constructor(coffeeBeans: number) {
      this.coffeeBeans = coffeeBeans
    }

    fillCoffeeBeans(beans: number) {
      if (beans < 0) {
        throw new Error('value for beans should be greater than 0')
      }
      this.coffeeBeans += beans
    }

    clean() {
      console.log('cleaning the machine...🧼')
    }

    private grindBeans(shots: number) {
      console.log(`grinding beans for ${shots}`)
      if (this.coffeeBeans < shots * CoffeeMachine.BEANS_GRAMM_PER_SHOT) {
        throw new Error('Not enough coffee beans!')
      }
      this.coffeeBeans -= shots * CoffeeMachine.BEANS_GRAMM_PER_SHOT
    }

    private preheat(): void {
      console.log('heating up... 🔥')
    }

    protected abstract extract(shots: number): CoffeeCup

    makeCoffee(shots: number): CoffeeCup {
      this.grindBeans(shots)
      this.preheat()
      return this.extract(shots)
    }
  }

  class CaffeLatteMachine extends CoffeeMachine {
    serialNumber: string
    constructor(beans: number, public readonly serialNumber: string) {
      super(beans)
    }
    private steamMilk(): void {
      console.log('Steaming some milk... 🥛')
    }

    protected extract(shots: number): CoffeeCup {
      this.steamMilk()
      return {
        shots,
        hasMilk: true,
      }
    }
  }

  class SweetCoffeeMaker extends CoffeeMachine {
    protected extract(shots: number): CoffeeCup {
      return {
        shots,
        hasSugar: true,
      }
    }
  }

  const machines: CoffeeMaker[] = [
    new CaffeLatteMachine(16, '1'),
    new SweetCoffeeMaker(16),
    new CaffeLatteMachine(16, '2'),
    new SweetCoffeeMaker(16),
  ]
  machines.forEach(machine => {
    console.log('-------------------------')
    machine.makeCoffee(1)
  })
}
```

<br>

### 8.7 멤버 접근성

- public(default) : 모든 곳에서 누구나 접근 가능
- private : 클래스 내부에서만 접근 가능
- protected : 클래스 내부 또는 서브 클래스에서만 접근 가능
- static : 정적 프로퍼티, 정적 메서드
- readonly : 읽기 전용

<br>

## Chapter 9. 타입 제한자 Type Assertions

- type assertion 은 되도록 안쓰는게 좋다.(안쓰게 코드를 짜는게 좋다)

```ts
function jsStrFunc(): any {
  return 5
}

const result = jsStrFunc()

console.log((result as string).length) // number타입..
console.log((<string>result).length) // 에러를 안잡아줌

const button = document.querySelector('btn')! // ? 와 반대개념 !
button.addEventListener('click', () => console.log(1))
```

<br>

### 9.1 top 타입 (any, unknown)

- bottom 타입은 never
- 시스템에서 가능한 모든 값을 나타내는 타입입니다.
- 모든 다른 타입의 값은 타입이 top인 위치에 제공될 수 있습니다.
- 즉, 모든 타입은 top 타입에 할당할 수 있습니다.

<br>

### 9.1.1 any 다시 보기

- "any는 타입 검사를 수행하지 않도록 명시적으로 지시하다는 문제점을 갖습니다."
- "타입스크립트의 유용성이 줄어듭니다."

```ts
function double(number: any) {
  return number * number
}

double(10)
double('string') // 오류 없음
```

<br>

### 9.1.2 unknown

- 모든 객체를 unknown 타입의 위치로 전달할 수 있다.
- 타입스크립트는 unknown 타입의 값을 훨씬 더 제한적으로 취급한다.
- 가능하다면 any 대신 unknown을 사용하길 추천

```ts
function processInput(input: unknown): string {
  // Perform type checking using a type guard
  if (typeof input === 'string') {
    return input.trim().toUpperCase() // Allowed since input is now of type string
  }
  if (typeof input === 'number') {
    return input.toFixed(2) // Allowed since input is now of type number
  }
  if (input instanceof Array) {
    return JSON.stringify(input) // Allowed since input is now of type Array
  }
  throw new Error('Invalid input type.')
}

const input1: unknown = 'Hello, TypeScript!'
const input2: unknown = 42
const input3: unknown = [1, 2, 3]

console.log(processInput(input1)) // Outputs: HELLO, TYPESCRIPT!
console.log(processInput(input2)) // Outputs: 42.00
console.log(processInput(input3)) // Outputs: [1,2,3]
```

<br>

### 9.2 타입 서술어

- 로직을 함수로 감싸면 위처럼 타입을 제한된 검사로 좁힐 수 없게 됩니다.

```ts
function isNumberOrString(value: unknown) {
  return ['number', 'string'].includes(typeof value)
}

function logValueIfExists(value: number | string | null | undefined) {
  if (isNumberOrString(value)) {
    // 타입스트립트는 isNumberOrString 함수의 반환 타입이 Boolean 것만 추론 가능
    // value 값의 타입은 number | string 으로 좁혀지지 않는다.
    value.toString() // 'value' is possibly 'null' or 'undefined'
  }
}
```

- 타입 서술어(type predicate) `is`
- Syntax: 매개변수 이름 + `is` + 타입

```ts
function typePredicate(input: WideType): input is NarrowType
```

```ts
interface Fish {
  swim: () => void
}

interface Bird {
  fly: () => void
}

function isFish(animal: Fish | Bird): animal is Fish {
  // is 쓰면 리턴 타입이 Boolean
  // true를 리턴하면 animal이 Fish로 좁혀짐
  return 'swim' in animal
}

function move(animal: Fish | Bird): void {
  if (isFish(animal)) {
    // true 라서 animal이 Fish로 좁혀짐
    animal.swim() // TypeScript now knows animal is of type Fish
  } else {
    animal.fly() // TypeScript now knows animal is of type Bird
  }
}

const fish: Fish = { swim: () => console.log('Swimming...') }
const bird: Bird = { fly: () => console.log('Flying...') }

move(fish) // Outputs: Swimming...
move(bird) // Outputs: Flying...
```

<br>

### 9.3 타입 연산자

### 9.3.1 keyof

- "자바스크립트 객체는 일반적으로 string 타입인 동적값을 사용하여 검색된 멤버를 갖습니다."
- "string 같은 포괄적인 원시 타입을 사용하면 컨테이너 값에 대한 유효하지 않은 키가 허용됩니다"

```ts
interface Person {
  name: string
  age: number
  occupation: string
}

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]
}

const person: Person = {
  name: 'Alice',
  age: 30,
  occupation: 'Software Engineer',
}

const myName = getProperty(person, 'name') // Type: string
const age = getProperty(person, 'age') // Type: number
const occupation = getProperty(person, 'occupation') // Type: string
const invalid = getProperty(person, 'invalidKey') // Error: Argument of type '"invalidKey"' is not assignable to parameter of type 'keyof Person'
```

<br>

### 9.3.2 typeof

- "제공되는 값의 타입을 반환합니다."
- 자바스크립트의 typeof 연산자와 다르다.
- 타입스크립트에서만 사용할 수 있으면 컴파일된 자바스크립트 코드에 나타나지 않는다.
- 우연히 같은 단어를 사용할 뿐이다.

```ts
const person = {
  name: 'Alice',
  age: 30,
  occupation: 'Software Engineer',
}

type PersonType = typeof person

function greet(person: PersonType): string {
  return `Hello, ${person.name}! You're a ${person.occupation}.`
}

const anotherPerson: PersonType = {
  name: 'Bob',
  age: 28,
  occupation: 'Data Scientist',
}

console.log(greet(anotherPerson)) // Output: "Hello, Bob! You're a Data Scientist."
```

### keyof typeof

```ts
const person = {
  name: 'Alice',
  age: 30,
  occupation: 'Software Engineer',
}

type PersonType = typeof person
type PersonKeys = keyof typeof person

function getProperty(obj: PersonType, key: PersonKeys): PersonType[PersonKeys] {
  return obj[key]
}

const anotherPerson: PersonType = {
  name: 'Bob',
  age: 28,
  occupation: 'Data Scientist',
}

const myName = getProperty(anotherPerson, 'name') // Type: string
const age = getProperty(anotherPerson, 'age') // Type: number
const occupation = getProperty(anotherPerson, 'occupation') // Type: string

// The following line would result in a compile-time error, as "invalidKey" is not a key of the PersonType object.
// const invalid = getProperty(anotherPerson, "invalidKey");
```

### 9.4 타입 어서션 - Type Assertion

- 타입 캐스트 (type cast) 라고도 부름
- 다른 타입을 의미하는 값의 타입 다음에 `as` 키워드를 배치합니다.
- "모범 사례는 가능한 한 타입 어서션을 사용하지 않는 것입니다."

```ts
// Using type assertion with an unknown type
function processInput(input: unknown): string {
  if (typeof input === 'string') {
    return input.toUpperCase() // No need for type assertion as TypeScript can infer the type
  }

  // Type assertion that input is a number
  const numericInput = input as number
  return numericInput.toFixed(2)
}

console.log(processInput('hello')) // Output: "HELLO"
console.log(processInput(42)) // Output: "42.00"
```

<br>

### 9.4.1 포착된 오류 타입 어서션

- catch 블록에서 포착된 오류가 어떤 타입인지 아는 것을 일반적으로 불가능
- 만약 코드 영역이 Error 객체를 발생시킬거라 확신한다면 타입 어셔션 사용 가능

```ts
class CustomError extends Error {
  public customErrorCode: number

  constructor(message: string, customErrorCode: number) {
    super(message)
    this.customErrorCode = customErrorCode
  }
}

function processInput(input: string): void {
  try {
    if (input.length === 0) {
      throw new CustomError('Input should not be empty', 1001)
    }

    // Processing the input...
  } catch (error) {
    // Type assertion that error is an instance of CustomError
    if (error instanceof CustomError) {
      console.error(`Custom error (${error.customErrorCode}): ${error.message}`)
    } else if (error instanceof Error) {
      // Handle other error types or rethrow the error
      console.error(`General error: ${error.message}`)
      // console.error(`General error: ${(error as Error).message}`)
    }
  }
}

processInput('') // Output: "Custom error (1001): Input should not be empty"
```

<br>

### 9.4.2 non-null 어서션 `!`

- "이론적으로만 null 또는 undefined를 포함할 수 있는 변수에서 null과 undefined를 제거할 때 타입 어서션을 주로 사용합니다."
- `!` 를 사용
- 정말 확신하는 경우에만 사용해야 한다.
- 아니면 적절한 검사를 수행하거나 `?.`을 사용하는 것도 방법이다.

```ts
type Person = {
  name: string
  age: number
  occupation?: string
}

function getOccupation(person: Person): string {
  // Using non-null assertion operator '!' to tell TypeScript that person.occupation is not null or undefined
  return person.occupation!
}

const alice: Person = {
  name: 'Alice',
  age: 30,
  occupation: 'Software Engineer',
}

const bob: Person = {
  name: 'Bob',
  age: 28,
}

console.log(getOccupation(alice)) // Output: "Software Engineer"
console.log(getOccupation(bob)) // Output: undefined (runtime error if you try to use the result)
```

### 9.4.3 타입 어서션 주의사항

### 어서션 vs 선언

### 9.5 const 어서션

- "const 어서션은 배열, 원시 타입, 값, 별칭 등 모든 값을 상수로 취급해야 함을 나타내는 데 사용합니다."
- 배열은 `읽기전용튜플`로 취급
- 리터럴은 일반적인 원시 타입과 동등하지 않고 리터럴로 취급
- 객체의 속성은 읽기 전용으로 간주

```ts
// Using const assertion to create a readonly tuple with fixed literal types
// 'as const' 어설션이 오류 없이 할당되도록 허용하는 'readonly' 키워드를 사용하여 'rgb' 변수를 읽기 전용 튜플로 정의합니다.
const rgb: readonly [number, number, number] = [0, 255, 128] as const

// Using const assertion to create a readonly object with fixed literal types
const theme = {
  primaryColor: '#007bff',
  secondaryColor: '#6c757d',
  backgroundColor: '#ffffff',
} as const

// Using const assertion with a string literal to create a string literal type
const appName = 'MyApp' as const

function setAppName(name: typeof appName) {
  // The function only accepts the exact string "MyApp" as its argument
}

setAppName('MyApp') // This works
setAppName('AnotherApp') // This will cause a compile-time error

// Attempting to modify a const assertion object will result in a compile-time error
theme.primaryColor = '#ff0000' // Error: Cannot assign to 'primaryColor' because it is a read-only property

// Attempting to modify a const assertion tuple will result in a compile-time error
rgb[0] = 128 // Error: Index signature in type 'readonly [0, 255, 128]' only permits reading
```
