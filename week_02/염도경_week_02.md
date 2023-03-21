## 필수 매개변수

> 함수에 선언된 모든 매개변수가 필수라고 가정한다

```ts
function singTwo(first: string, second: string) {
  console.log(`${first} / ${second}`);
}
```

위의 singTwo 함수는 두 개의 매개변수가 필요하고, 두 개의 인수가 들어와야 한다.

## 선택적 매개변수

> 함수 내부의 인숫값은 undefined로 기본설정 된다.

- 타입 애너테이션 : 앞에 ?를 추가하면 매개변수가 선택적이게 될 수 있다.

```ts
function announceSong(song: string, singer?: string) {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
}

announceSong("mango"); //ok
announceSong("mango", undefined); //ok
announceSong("mango", "sia"); //ok
```

- singer는 string값이거나 undefined가 될 수 있다.
- 함수에서 사용되는 선택적 매개변수는 `마지막` 매개변수 여야 한다.

## 기본 매개변수

- 선택적 매개변수를 선언할 때 `=`로 선언해줄 수 있다.

```ts
function rateSont(song: string, rating = 0) {
  console.log(`${song} get ${rating}`);
}

rateSong("mango"); //ok
rateSong("mango", 5); //ok
rateSong("mango", undefined); //ok
```

## void, never

### void

- 반환 타입이 void인 함수는 값을 반환하지 않을 수 있다.

```ts
function logSong(song: string | undefined): void {
  if (!Song) {
    return; //ok
  }
  console.log(`${songs}`);

  return true; //Error
}
```

위처럼 void를 사용하면 반환하는 값은 무시된다.

```ts
let songLogger: (song: string) => void;

songLogger = (song) => {
  console.log(`${songs}`);
};

songLogger("Heart of glass"); //true
```

위의 songLogger 변수는 song: string을 받고, 값을 반환하지 않는다.

- void 타입은 js가 아닌 함수의 반환 타입을 선언하는데 사용하는 타입스크립트 키워드이다.

### never

- 함수를 절대 반환하지 않도록 의도하려면 명시적 : never 타입 애너테이션을 추가한 후,
  해당 함수를 호출한 후 모든 코드가 실행되지 않음을 나타낸다.

---

# 배열

```ts
const mango = ["dog", "love"];

mango.push("hi"); //ok

mango.push(true); //error
```

- 위의 mango 배열은 초기에 string 값을 포함한다는것을 알고 있으므로 이후에 string 타입은 포함하지만, 다른 데이터는 허용하지않는다.

- 배열 저장하기 위한 변수도 초깃값이 필요하지 않는다.

- 변수는 처음에 undefined로 시작해서 나중에 배열 값을 받는다.

```ts
let arrayOfNumbers: number[];

arrayOfNumbers = [2, 4, 5];

let createStrings: () => string[]; //string배열을 반환하는 함수
let stringCreaters: () => string[]; //각각의 string을 반환하는 함수 배열
```

- 초기에 빈배열로 설정된 변수에서 타입 애너테이션을 포함하지 않으면, 타입스크립트는 배열은 any[]로 취급하고 모든 콘텐츠를 받을 수 있습니다.

- 이렇게 any타입이 되면, 타입스크립트는 검사 목적을 부분적으로 무효화 하기 때문에, ts는 값의 타입을 알게 할 때 가장 잘 동작한다.

### 다차원 배열

```ts
let arrayOfArrayOfNumbers: number[][];
```

위처럼 나타낼 수 있다.

### 튜플

- 튜플 : 고정된 크기의 배열 : 각 인덱스에 알려진 특정 타입을 가진다.

- 보통 튜플 배열 구조 분해 할당을 자주 사용한다.

```ts
let [year, warrior] = Math.random() > 0.5 ? [340, "MAngo"] : [1828, "Yeom"];
//year type은 number, warrior type은 string
```

## 인터페이스

- 타입 별칭 vs 인터페이스

```ts
type Mango = {
  born: number;
  name: string;
} //타입 별칭으로 구분

interface Mango ={
  born: number;
  name: string;
} //인터페이스로 구분

```

- 인터페이스는 속성 증가를 위해 병합할 수 있다 ( 내장된 전역 인터페이스 또는 npm 패키지와 같은 외부 코드를 사용할때 유용)
  인터페이스에서 타입스크립트 타입 검사기가 더 빨리 작동한다.
  가능하면 인터페이스 사용을 추천함.

- 인터페이스를 함수로 선언하는 두 가지 방법

  1. 메서드 구문 member():void
  2. 속성 구문 member: ()=> void
     +++++ 이 부분 좀 더 이해해야 할듯하다 +++++

- 인터페이스 확장
  - 인터페이스가 다른 인터페이스를 복사해서 선언할 수 있는 확장된 interface를 허용합니다.
  - 확장할 인터페이스 이름 뒤에 extends 키워드를 추가해서 확장인터페이스라는것을 표시합니다.

```ts
interface Writing {
  title: string;
}

interface Novella extends Writing {
  pages: number;
}
//Novella 인터페이스는 Writing을 확장하므로 Novella와 Writing의 모든 멤버가 있어야한다.

let myNovella: Novella = {
  title: "hi",
  pages: 14,
}; //OK

//위의 myNovella에서 멤버를 추가해도 안되고, pages, title 멤버 중에서 하나라도 빠지면 안된다.
```

이런식으로 여러 인터페이스를 확장하는 방식으로 사용하면 코드 중복을 줄이고 다른 영역에서 더 쉽게 사용이 가능하다.

- 인터페이스 병합 : 일반적으로는 자주 사용하지 않지만, 외부 패키지 또는 window같은 내장된 전역 인터페이스를 보강하는데 유용

```ts
interface Window {
  myEnvironment: string;
}

window.myEnvironment; // 타입 : string
```

- 이름이 충돌되는 멤버 ( 병합된 인터페이스는 타입이 같아야 한다)
