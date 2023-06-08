### 클래스 메서드

:any 타입을 기본으로 갖는다. string 타입의 단일 필수 매개변수를 갖는 greet 클래스 메서드를 가진 Greeter 클래스를 정의하는 코드

```ts
class Greeter {
  greet(name: string) {
    console.log(`${name}, do you stuff!`);
  }
}

new Greeter().greet("Miss Frizzle"); //ok

new Greeter().greet(); // Error
```

1개의 인자가 필요한데, 0개의 인자가 들어가서 에러가 나는 모습

- 클래스 생성자는 매개변수와 관련하여 `전형적인 클래스 메서드`처럼 취급된다.

```ts
class Greeter {
  constructor(message: string) {
    console.log(`${message}, do you stuff!`);
  }
}

new Greeter("this is message"); //ok

new Greeter(); // Error
```

위의 Greeted 생성자의 매개변수는 message: string으로 제공되어야 한다.

### 클래스 속성

ts에서 클래스의 속성을 읽거나 쓰려면 클래스에 명시적으로 선언해야 한다.
클래스 속성은 인터페이스와 동일한 구문을 사용해 선언.
클래스 속성 이름 뒤에는 선택적으로 타입 애너테이션이 붙는다.

```ts
class FieldTrip {
  destination: string;

  constructor(destination: string) {
    this.destination = destination; // ok
    console.log(`we're going to ${this.destination}`);
  }
}

this.nonexistent = destination; // Error
```

nonexistent 같은 존재하지 않는 멤버에 접근하려고 하면 오류가 발생한다.

### 함수 속성

```ts
class WithMethod {
  myMethod() {}
}

new WithMethod().myMethod === new WithMethod().myMethod; //true
```

위의 WithMethod는 모든 인스턴스가 참조할 수 있는 myMethod 메서드를 선언한다.

-

```ts
class WithProperty {
  myProperty: () => {};
}

new WithMethod().myMethod === new WithMethod().myProperty; //false
```

위의 WithProperty class는 myProperty인 단일 속성을 포함하며 각 클래스 인스턴스에 대해 다시 생성되는 () => void 타입이다.

---

```ts
class WithPropertyParameters {
  takesParameters = (input: boolean) => (input ? "Yes" : "No");
}

const instance = new WithPropertyParameters();

instance.takesParameters(true); //ok
instance.takesParameters(123); //error
```

위의 WithPropertyParameters 클래스는 타입이 (input : boolean( => string인 takesParameters 속성을 가진다.

---

#### 초기화 검사

엄격한 컴파일러 설정이 활성화된 상태에서 ts는 undefined 타입으로 선언된 각 속성이 생성자에서 할당 되었는지 확인한다.
이와 같은 엄격한 초기화 검사는 클래스 속성에 `값을 할당하지 않았을때` 와 같은 실수를 예방할 수 있어서 유용하다.

```ts
class WithValue {
  immediate = 0; //ok
  later: number; //ok (밑쪽의 constructor에서 할당 )
  mayBeUndefined: number | undefined; //ok (undefined 허용)
  unused: number; //Error

  constructor() {
    this.later = 1;
  }
}
```

#### 읽기 전용 속성

인터페이스와 마찬가지로 클래스도 선언된 속성 앞에 readonly 키워드를 추가해 속성을 읽기 전용으로 선언한다. readonly 키워드는 타입 시스템에만 존재하며, js로 컴파일할 때 삭제된다.

### 타입으로서의 클래스

```ts
class Teacher {
  sayHello() {
    console.log("Take chances, make mistakes, get messy!");
  }
}

let teacher: Teacher;

teacher = new Teacher(); //ok
teacher = "Wahoo!"; //Error
```

- Teacher 클래스의 이름은 teacher 변수의 주석을 다는데에 사용된다.
  teacher 변수에는 Teacher 클래스의 자체 인스턴스처럼 Teacher 클래스에 할당할 수 있는 값만 할당해야 함을 알려준다.

### 클래스 확장

```ts
class Teacher {
  teach() {
    console.log("the test");
  }
}

class StudenTeacher extends Teacher {
  lear() {
    console.log("i have open mind");
  }
}

const teacher = new StudentTeacher();

teacher.teach(); //ok 기본 클래스에 정의
teacher.learn(); //ok 하위 클래스에 정의

teacher.other(); //Error
```

Teacher는 StudentTeacher 하위 클래스의 인스턴스에서 사용할 수 있는 teach 메서드를 선언하고 있다.

### 타입 제한자

top 타입 : 시스템에서 가능한 모든 값을 나타내는 타입

모든 다른 타입의 값은 타입이 top인 위치에 제공될 수 있다.

즉, 모든 타입은 top 타입에 할당할 수 있다.

any 타입은 모든 타입의 위치에 제공될 수 있다는 점에서 top타입처럼 작동할 수 있다.

```ts
let anyValue: any;
anyValue = "Mango"; //ok
anyValue = 123; //ok

console.log(anyValue); //ok
```

하지만, any는 안정성 부족을 가지고 있기 때문에, 해당 값에 대한 유용성이 줄어든다.

```ts
function greetComedian(name: any) {
  console.log(`${name.toUpperCase()}`); //오류없음
}

greetComdeian({ name: "Yeom" }); //Error
```

위처럼 함수를 호출하고 값을 할당할 때 오류가 나온다.

#### 진정한 top 타입 = unknown

모든 객체를 unknown 타입의 위치로 전달 할 수 있다는 점에서 any와 유사하지만,
unknown 타입과 any 타입의 주요 차이점은 ts가 unknown의 타입을 훨씬 더 `제한적`으로 취급한다는 것!

    - 타입스크립트는 unknwon 타입 값의 속성에 직접 접근할 수 없습니다.
    - unknown 타입은 top 타입이 아닌 타입에는 할당할 수 없습니다.

```ts
function greetComedian(name: unknown) {
  console.log(`${name.toUpperCase()}`); //Error
}
```

name의 타입이 unknown일 때, 유일한 방법은 instanceof나 typeof 처럼 타입이 제한된 경우이다.
unknown에서 string으로 타입을 좁히기 위한 typeof사용 예제를 보자!

```ts
function greetComedianSafety(name: unknown) {
  if (typeof value === "string") {
    console.log(`${name.toUpperCase()}`);
  } else {
    console.log("Well,i'm off");
  }
}

greetComedianSafety("Betty White"); //Logs:4
greetComedianSafety({}); //로그 없음
```

위와 같이 instanceof 와 typeof 로 타입을 줄이는 법 말고도, 로직을 함수로 감싸고 타입을 줄이는 방법은 어떨까?
하지만 이런 방법은 마지막에 불리언값으로 받게 만들게 되는데, 이렇게 하면 ts는 그저 불리언값만 인식하게 되어서 타입을 좁히기 위함이란건 알 수 없게 된다.

그래서 특정 타입인지 여부를 나타내기 위해 boolean 값을 반환하는 함수를 위한 특별한 구문이 있는데, 이것을 `타입 서술어`라고 한다.

타입 서술어의 반환 타입은 매개변수의 이름, is 키워드, 특정 타입으로 선언할 수 있다.

### 타입 연산자

### 타입 어서션

- 이 타입이 특정 타입임을 단언합니다 라고 말하는 것

오직 컴파일 과정에서만 사용됨

<br><br><br>
간단한 코드 예제

```ts
let user: string = "kim";
let age: number | undefined = undefined;
let married: boolean = false;
let 철수: (string | number | boolean | undefined)[] = [user, age, married];
```

```ts
let 학교 = {
    score : (number | boolean)[],
    teacher : string,
    friend : string | string[]
}
= {
    score : [100, 97, 84],
    teacher : 'Phil',
    friend : 'John'
}
학교.score[4] = false;
학교.friend = ['Lee' , 학교.teacher]
```
