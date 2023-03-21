클래스 

클래스 메서드 
```ts
class Greeter {
    greet(name: string) {
        console.log('${name}, do your stuff!');
    }
}
new Greeter().greet("Miss Frizzle");

new Greeter().greet();
// error : name:string이 주어져 있지 않기 때문
```

클래스 속성 
```ts
class FieldTrip {
    destination: string;

    constructor(destination: string) {
        this.desination = destination;
        console.log('we re going to ${this.destination}!');

        this.nonexistent = destination;
        // error : nonexistent가 선언되어 있지 않기 떄문 
    }
}
```

함수 속성
```ts
class WithMethod {
    myMethod() {}
}

new WithMethod().myMethod === new WithMethod().myMethod; // true
```

```ts
class WithPropertyParameters {
    takesParameters = (input: boolean) => input ? "Yes" : "No";
}

const instance = new WithPropertyParameters();

instance.takesParameters(true); // true
instance.takesParameters(123); // error : boolean타입이 아닌 number는 안된다. 
```

선택적 속성 
```ts
class MissingInitializar {
    property? : string;
}

new MissingInitializar().property?.length; //true
new MissingInitializar().property.length; //error
```

읽기 전용 속성
```ts
class Quote {
    readonly text : string;

    constructor(text: string) {
        this.text = ;
    }

    emphasize() {
        this.text += "!"; 
        //error : text 속성은 생성자에서는 값이 지정되지만 다른 곳에서 값을 지정하려고 하면 타입 오류 발생
    }
}

const quote = new Quote (
    "There is a inside every student"
);

Quote.text = "Ha!";
// error : text 속성은 생성자에서는 값이 지정되지만 다른 곳에서 값을 지정하려고 하면 타입 오류 발생
```

클래스 확장 
```ts
class Teacher {
    teach() {
        console.log();
    }
}
class StudentTeacher extends Teacher {
    learn() {
        console.log();
    }
}

const teacher = new StudentTeacher();

teacher.teach(); // true
teacher.learn(); // true

teacher.other(); // error: 선언 되지 않은 other은 출력되지 않는다. 
```

재정의된 생성자 
```ts
class GradeAnnouncer {
    message : string;

    constructor(grade: number) {
        this.message = grade >= 65 ? "Maybe next time..." : 'You pass';
    }
}

class PassingAnnouncer extends GradeAnnouncer {
    constructor() {
        super(100) // true
    }
}

class FailingAnnouncer extends GradeAnnouncer {
    constructor() {
        // error : PassingAnnouncer은 number 인수를 사용해 기본 클래스인 GradeAnnouncer의 생성자를 올바르게 호출하지만 FailingAnnouncer는 기본 생성자를 올바르게 호출하지 않아 타입 오류 발생 
    }
}
```

멤버 접근성 

public : 모든 곳에서 누구나 접근 가능
protected : 클래스 내부 또는 하위 클래스에서만 접근 가능
private : 클래스 내부에서만 접근 가능 

타입 제한자 

any 타입 
- 모든 타입의 위치에 제공될 수 있다는 점에서 top 타입처럼 작동할 수 있습니다. 
- 일반적으로 console.log의 매개변수와 같이 모든 타입의 데이터를 받아들이는 위치에서 사용합니다. 

```ts
let anyValue : any;
anyValue = "Lucky";
anyValue = 123;

console.log(anyValue); 
```

unknown 타입 
- 진정한 top 타입 
- 모든 객체를 unknown 타입의 위치로 전달할 수 있다는 점에서 any타입과 유사합니다. 
- 타입스크립트는 unknown 타입 값의 속성에 직접 접근할 수 없습니다. 
- unkown타입은 top 타입이 아닌 타입에는 할당할 수 없습니다. 

타입 연산자 

keyof 

```ts
interface Ratings {
    audience : number;
    critics : number;
}

function getRating(ratings: Ratins, key: string): number {
    return retings[key];
    // error : 타입 string은 Ratings 인터페이스에서 속성으로 허용되지 않는 값을 허용하고, Ratings는 string 키를 허용하는 인덱스 시그니처를 선언하지 않습니다. 
}

const ratings: Ratings = { audience: 66, critic: 84 };

getRating(ratings, 'audience'); //true
 
getRating(ratings, 'not valid'); // 허용되지만 사용하면 안된다. 
```

typeof 
```ts
Const original = {
    medium: "movie",
    title: "Mean Girls",
};

let adaptation : typeof original;

if(Math.random() > 0.5) {
    adaptation = { ...original, medium: "play"}; // ok
} else {
    adaptation = { ...original, medium: 2 }; //error : medium는 string
}
```

타입 어서션
- 값의 타입에 대한 타입 시스템의 이해를 재정의하기 위한 구문

```ts
const rawData = '["grace", "frankie"]';

JSON.parse(rawData); // any

JSON.parse(rawData) as string[]; // string[]

JSON.parse(rawData) as [string, string]; // [string, string]

JSON.parse(rawData) as ["grace", "frankie"]; // ["grace", "frankie"]
```

non-null 어서션

```ts
let maybeDate = Math.random() > 0.5 ? undefined : new Date();
// 타입 유추 : Date | undefined

maybeDate as Date;
// 타입이 Date라고 간주됨

maybeDate!;
// 타입이 Date라고 간주됨
```

어서션 VS. 선언
- 변수 타입을 선언하기 위해 타입 애너테이션을 사용하는 것과 초깃값으로 변수 타입을 변경하기 위해 타입 어서션을 사용하는 것 사이에는 차이가 있습니다. 
- 변수의 타입 애너테이션과 초깃값이 모두 있을 때, 타입스크립트의 타입 검사기는 변수의 타입 애너테이션에 대한 변수의 초깃값에 대해 할달 가능성 검사를 수행합니다.
- 타입 어서션은 타입스크립트에 타입 검사 중 일부를 건너뛰도록 명시적으로 지시합니다. 

const 어서션 

as const 세가지 규칙
1. 배열은 가변 배열이 아니라 읽기 전용 튜플로 취급됩니다. 
2. 리터럴은 일반적인 원시 타입과 동등하지 않고 리터럴로 취급됩니다. 
3. 객체의 속성은 읽기 전용으로 간주됩니다. 

```ts
// 타입 : (number | string)[]
[0, ''];

// 타입 : readonly [0, '']
[0, ''] as const 
```

리터럴에서 원시타입으로 
```ts
// 타입 : () => string
const getName = () => "Maria Bamford";

// 타입 : () => "Maria Bamford"
const getNameConst = () => "Maria Bamford" as const;
```
