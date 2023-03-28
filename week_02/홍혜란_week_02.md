7. 인터페이스

인터페이스로 작성한 구문 예시 
```ts
interface Poet {
    born : number;
    name : string;
}
```

- 인터페이스는 클래스가 선언된 구조의 타입을 확인하는데 사용할 수 있지만 타입 별칭은 사용할 수 없습니다. 
- 일반적으로 인터페이스에서 타입스크립트 타입 검사기가 더 빨리 작동합니다. 
- 인터페이스는 이름 없는 객체 리터럴의 별칭이 아닌 이름 있는 객체로 간주되므로 어려운 특이 케이스에서 나타나는 오류 메시지를 좀 더 쉽게 읽을 수 있습니다. 

7.2 속성 타입 

7.2.1 선택적 속성
객체 타입과 마찬가지로 모든 객체가 필수적으로 인터페이스 속성을 가질 필요는 없습니다. 
타입 애너테이션 : 앞에 ?를 사용해 인터페이스의 속성이 선택적 속성임을 나타낼 수 있습니다. 

```ts
interface Book {
    author?: string;
    pages: number;
}

const ok : Book = {
    author : "Rita Dove",
    pages : 80;
} // ok

const missing : Book = {
    pages : 80;
}
```
Book 인터페이스는 필수 속성 pages와 선택적 속성 author를 갖습니다. Book 인터페이스를 사용하는 객체에 필수 속성만 제공된다면 선택적 속성은 제공되거나 생략할 수 있습니다. 

7.2.2 읽기 전용 속성 
경우에 따라 인터페이스에 정의된 객체의 속성을 재할당하지 못하도록 인터페이스 사용자를 차단하고 싶습니다. 
타입스크립트는 속성 이름 앞에 readonly 키워드를 추가해 다른 값으로 설정될 수 없음을 나타냅니다. 
이러한 readonly 속성은 평소대로 읽을 수 있지만 새로운 값으로 재할당하지 못합니다. 

예) page인터페이스의 text 속성에 접근하면 string을 반환하지만, text에 새로운 값을 할당하면 타입 오류가 발생합니다. 
```ts
interface Page {
    readonly text : string;
}

function read(page: Page) {
    console.log(page.text);
    //ok

    page.text += "!";
    // error: Cannot assign to 'text' because it is a read-only property.
}
```

readonly 제한자는 타입 시스템에만 존재하며 인터페이스에서만 사용할 수 있습니다. 
readonly 제한자는 객체의 인터페이스를 선언하는 위치에서만 사용되고 실제 객체에는 적용되지 않습니다. 

예) 쓰기 가능한 속성을 readonly 속성에 할달할 수 있으므로 pageIsh는 Page로 사용할 수 있습니다. 
```ts
const pageIsh = {
    text : "Hello",
};

pageIsh.text += "!"; //ok

read(pageIsh); //ok
```
pageIsh는 Page객체가 아니라, text가 있는 유추된 객체 타입입니다. 

readonly 인터페이스 멤버는 코드 영역에서 객체를 의도치 않게 수정하는 것을 막는 편리한 방법입니다. 
그러나 readonly는 타입 시스템 구성 요소일 뿐 컴파일된 자바스크립트 출력 코드에는 존재하지 않습니다. 
readonly는 단지 타입스크립트 타입 검사기를 사용해 개발 중에 그 속성이 수정되지 못하도록 보호하는 역할을 합니다. 

7.2.3 함수와 메서드 

메서드 구문 : 인터페이스 멤버를 member(): void와 같이 객체의 멤버로 호출되는 함수로 선언
속성 구문 : 인터페이스 멤버를 member: () => void와 같이 독립 함수와 동일하게 선언

예) method와 property멤버는 둘 다 매개변수 없이 호출되어 string을 반환합니다. 
```ts
interface HasBothFunctionTypes {
    property: () => string;
    method(): string;
}

const hasBoth : HasBothFunctionTypes= {
    property: () => '';
    method() {
        return '';
    }
};

hasBoth.property(); 
hasBoth.method();
```

두가지 방법 모두 선택적 속성 키워드인 ? 를 사용해 필수로 제공하지 않아도 되는 멤버로 나타낼 수 있습니다. 
```ts
interface Optional {
    optionalProperty?: () => string;
    optionalMethod?() : string;
}
```

- 메서드는 readonly로 선언할 수 없지만 속성은 가능합니다. 
- 타입에서 수행되는 일부 작업은 메서드와 속성을 다르게 처리합니다. 

7.2.4 호출 시그니처 
인터페이스와 객체 타입은 호출 시그니처로 선언할 수 있습니다.
호출 시그니처는 값을 함수처럼 호출하는 방식에 대한 타입 시스템의 설명입니다. 
호출 시그니처가 선언한 방식으로 호출되는 값만 인터페이스에 할당할 수 있습니다.
즉, 할당 가능한 매개변수와 반환 타입을 가진 함수입니다. 
호출 시그니처는 함수 타입과 비슷하지만, 콜론(:) 대신 화살표 (=>)로 표시합니다. 

```ts
type FunctionAlias = (input: string) => number;

interface Callsignature {
    (input: string): number;
}

const typedFunctionAlias: FunctionAlias = (input) => input.length; //ok

const typedCallSignature: CallSignature = (input) => input.length; //ok
```
호출 시그니처는 사용자 정의 속성을 추가로 갖는 함수를 설명하는 데 사용할 수 있습니다. 
타입스크립트는 함수 선언에 추가된 속성을 해당 함수 선언의 타입에 추가하는 것으로 인식합니다. 

7.2.5 인덱스 시그니처 
타입스크립트는 인덱스 시그니처 구문을 제공해 인터페이스의 객체가 임의의 키를 받고, 해당 키 아래의 특징 타입을 반환할 수 있음을 나타냅니다. 
일반 속성 정의와 유사하지만 키 다음에 타입이 있고 [i : string] : ... 과 같은 배열의 대괄호를 갖습니다. 

```ts
interface WordCounts {
    [i : string] : number;
}

const counts: WordCounts = {};

counts.apple = 0;
counts.banana = 1;

counts.cherry = false;
```

속성과 인덱스 시그니처 혼합 
인터페이스는 명시적으로 명명된 속성과 포괄적인 용도의 string 인덱스 시그니처를 한번에 포함할 수 있습니다. 
각각의 명명된 속성의 타입은 포괄적인 용도의 인덱스 시그니처로 할당할 수 있습니다. 
명명된 속성이 더 구체적인 타입을 제공하고, 다른 모든 속성은 인덱스 시그니처의 타입으로 대체하는 것으로 혼합해서 사용할 수 있습니다. 

속성과 인덱스 시그니처 혼합 사용방법
- 인덱스 시그니처의 원시 속성보다 명명된 속성에 대해 더 구체적인 속성 타입 리터럴을 사용한다. 
- 명명된 속성의 타입이 인덱스 시그니처에 할당될 수 있는 경우 타입스크립트는 더 구체적인 속성 타입 리터럴을사용하는 것을 허용합니다. 

예) ChapterStarts는 preface 속성은 0으로, 다른 모든 속성은 더 일반적인 number를 갖도록 선언한다. 
즉, ChapterStarts를 사용하는 모든 객체의 preface 속성은 반드시 0이어야 한다. 
```ts
interpace ChapterStarts {
    preface: 0;
    [i : string]: number;
}

const correctPreface: ChapterStarts = {
    preface: 0,
    night: 1,
    shopping: 5
};

const wrongPreface: ChapterStarts = {
    preface : 1,
    // error // 0이어야 함
};
```

숫자 인덱스 시그니처
타입스크립트 인덱스 시그니처는 키로 string 대신 number 타입을 사용할 수 있지만, 명명된 속성은 그 타입을 포괄적인 용도의 string 인덱스 시그니처의 타입으로 할당할 수 있어야 합니다. 

예)
```ts
interface MoreNarrowNumbers {
    [i: number]: string;
    [i: string]: string | undefinde; // ok
}

const mixesNumbersAndStrings: MoreNarrowNumbers = {
    0: '',
    key1: '',
    key2: undefined,
}

interface MoreNarrowStrings {
    [i: string]: string | undefinde;
    //error // string 할당 불가능 
    [i: string]: string;
}
```

7.3 인터페이스 확장 
타입스크립트는 인터페이스가 다른 인터페이스의 모든 멤버를 복사해서 선언할 수 있는 확장된 인터페이스를 허용합니다. 
확장할 인터페이스의 이름 뒤에 extends 키워드를 추가해서 다른 인터페이스를 확장한 인터페이스라는 걸 표시합니다. 
파생 인터페이스를 준수하는 모든 객체가 기본 인터페이스의 모든 멤버도 가져야 한다는 것을 알려줍니다. 

7.3.1 재정의된 속성
파생 인터페이스는 다른 타입으로 속성을 다시 선언해 기본 인터페이스의 속성을 재정의 하거나 대체할 수 있습니다.
재정의된 속성이 기본 속성에 할당되도록 강요합니다. 
파생 인터페이스 타입의 인스턴스를 기본 인터페이스 타입에 할당할 수 있습니다. 

해당 속성을 유니언 타입의 더 구체적인 하위 집합으로 만들거나 속성을 기본 인터페이스의 타입에서 확장된 타입으로 만들기 위해 사용합니다. 

7.3.2 다중 인터페이스 확장
여러개의 다른 인터페이스를 확장해서 선언할 수 있습니다. 
extends 키워드 뒤에 쉼표를 붙여 (,) 사용합니다. 

7.4 인터페이스 병합 
인터페이스의 중요한 특징 중 하나는 서로 병합하는 능력입니다. 

7.4.1 이름이 충돌되는 멤버
병합된 인터페이스는 타입이 다른 동일한 이름의 속성을 여러번 선언할 수 없습니다.