### 함수 매개 변수
- 함슈으의 매개변수도 타입을 지정할 수 있다.
- 타입의 정보가 선언되지 않는다면 any타입으로 간주하며 매개변수의 타입은 무엇이든 될 수 있다.

```ts
function play(football) {
    console.log('Playing: ${football}!')};

```

### 필수 매개변수
- 함수에 선언되 모든 매개변수가 필수라고 가정하자.
- 함수가 잘못된 수의 인수로 호출되면, 타입스크립는 타입 오류의 형태롷 이의 제기를 할 수 있다.
- 타입스크립트에서는 매개변수의 타입과 개수를 정확하게 일치하는지 확인합니다.
```ts
function palayOne(first: string, second: string) {
    conssole.log('${first} / ${second}');
palyOne("sport and soccer");
  // Error: Exptected 2 arguments, but got 1.

playOne("team", "player");  // ok
playOne("team", "palyer", "ball");
  // Error: Expected 2 arguments, but got 3.
}
```

### 선택적 매개변수
- 함수 메개변수가 제공되지 않으면 함수 내부의 인숫값은 undefined 기본값으로 설정된다.
- 선택적 객체 타입 속성과 유사하게 타입 에너네이션의 : 앞에 ? 를 추가해 매개변수가 선택적이라고 표시한다.
- 함수 호출에 선택적 매개변수를 제공할 필요는 없습니다.
- 선택적 매개변수에는 항상 | undefined 가 유니언 타입으로 추가되어 있습니다.

```ts
function Play(football: string, palyer?: string) {
  conosle.log('Play: ${football)');
  
  if(palyer) {
    console.log('Player ${player}');
  }
}

Play("a"); // ok
Play("a", undefined);  // ok
Play("a", "b"); // ok
```

- 선택적 매개변수는 항상 암묵적으로 undefined가 될 수 있다.
- 선택적 매개변수는 값이 명시적으로 undefined 일지라도 항상 제공되어야 한다.

```ts
function Play(football: string, player: string | undefined) {
  Play("a");
  // Error: Expected 2 arguments but got 1.
  
  Play("a", undefined);   // ok
  Play("a", "b");  // ok
```

- 모든 선택적 매개변수는 마지막 매개변수여야 한다.
- 필수 매개변수 전에 선택적 매개변가 먼저 오면 오류가 발생한다.

```ts
function Play(player?: string, football: string) {}
// Error: A required parmeter cannot follow an optional parameter.
```

### 기본 매개 변수
- 선택적 매개변수에는 기본적을 값이 제공되기 때문에 해당 타입스크립트 타입에는 암묵적으로 함수 내부에 | undefined 유니언 타입이 추가된다.
- 함수의 매개변수에 대해 인수를 누락하거나 undefined 인수를 사용해서 호출하는 것을 하용한다.

```ts
function Play(football:string, score = 0) {
  console.log('${football} gets ${score} / star!');
}
              
Play("a");   //ok
Play("a", 2);  // ok
Play("a", undefined);  // ok
Play("a", "7").   
// Error : Argument of type "7" is not assignable to 
// Parameter of type 'number | undefined'.
```
### 나머지 매개변수
- 자바스크립트이 일부 함수는 임의의 수의 인수로 호출할 수있도록 만들어진다.
- spread operator는 함수 선언의 마지막 매개변수에 위치하고, 해당 매개변수에서 시작해 함수에 전달된 '나머지' 인수가 모두 단일 배열에 저장되어야 한다.
- 인수 배열을 나나태내기 위해 끝에 [] 구문이 추가된다는 점만 다르다.

```ts
function allSongs(singer: string, ...songs: string[]) {
  for (const song of songs) {
    console.log('${song}, by ${singer}');
  }
}

allSongs("a"); //ok
allSongs("a", "b", "c");  //ok
allSongs("a", 3000);  
// Error : Argument of type 'number' is not assignable to parameter of type 'string'
```

### 반환 타입
- 함수가 반환할 수 있는 가능한 모든 값을 이해하면 함수가 반환하는 타입을 알 수 있다.
- 함수에 다른 값을 가진 여러 개의 반환문을 포함하고 있다면, 타입스크립트는 반환 타입을 가능한 모든 반환 타입의 조합으로 유추할 수 있다.

```ts
function getSongs(s: string[], n: number) {
  return n < s.length ? s[n]: undefined
}
```

### 명시적 반환 타입
- 변수와 마찬가지로 타입 애너테이션을 사용해 함수의 반환 타입을 명시적으로 선언하지 않는 것이 좋다. 
- 함수에서 반환 타입을 명시적으로 선언하는 방식이 매우 유용할 때가 종종 있다.

    - 가능한 반환값이 많은 함수가 항상 동일한 타입의 값을 반환하도록 강제한다.
        - 타입스크립트는 재귀 함수의 반환 타입을 통해 타입을 유추하는 것을 거부한다.
        - 수백 개의 이상의 타입스크립트 파일이 있는 매우 큰 프로젝트에서 타입스크립트 타입검사 속도를 높일 수 있다.
        
- 함수 선언 반환 타입 애너테이션은 매개변수 목록이 끝나는 ) 다음에 배치되고, 함수 선언의 경우는 { 앞에 위치한다.

```ts
function btsSong(songs: string[], count=0): number{
  return songs.length ? btsSong(songs.slice(1), count+1) : count;
}

function btsSong = (songs: string[], count = 0): number => 
  songs.length ? btsSong(songs.slice(1), count+1) : count;
  
```

- 함수의 반환 타입으로 할당할 수 없는 값을 반환하는 경우는 타입스크립트는 할당가능성 오류를 표시한다.

```ts
function btsDebutDate(song: string):
Date | undefined {
  swich (song) {
    case "2cool4skool";
      return new Date("June 13, 2013");  //ok
    
    case "save me";
      return "unkonwn";
    // Error: Type 'string' is not assignable to type 'Date'
    default:
      return undefined;  // ok
  }
}

```

### 함수 타입
- 함수를 값으로 전달 할 수 있고, 함수를 가지기 위한 매개변수 또는 변수의 타입을 선언하는 방법이 필요하다.
- 함수타입구문은 함수와 유사하지만 함수 본문 대신 타입이 있다.
``` ts
const nothing: () => string;  // 매개변수가 없고 string 타입을 반환

const input:(songs: string[], count?: number) => number;
// string[]매개변수와 count 선택적 매개변수 및 number 값을 반환하는 함수를 설명
```

- 함수 타입은 콜백 매개변수(함수로 호출되는 매개변수)를 설명하는데 자주 사용!!
```ts
const songs = ['Juice", "Sake It Off", "what's up"];
function runSongs(getSongAt: (index: number) => string) {
  for(let i = 0; i < songs.length; i += 1){
    console.log(getSongsAt(i));
  }
}

function getSongAt(index: number) {
  return '${songs[index]}';
}
runOnSongs(getSongAt);  // ok

function logSong(song: string) {
  return '${song}';
} 
runOnSongs(logSong);
//Error: Argument of type '(song: string) => string' is not
//  assignable to parameter of type '(index: number) => string'.
// Type of parameters 'song' and 'index' are incompatible.
// Type 'number' is not assignable to type 'string'
    
```

- `runOnSong` 에 대한 오류 메시지
    - 첫번째 들여쓰기 단께는 두 함수 타입을 출력한다.
        - 다음 들여쓰기 단계는 일치하지 않는 부분을 지정한다.
        - 마지막 들여쓰기 단계는 일치하지 않는 부분에 대한 정확한 할당 가능성 오류를 출력한다.
        
- 단계별로 제공하는 내용 정리하자면
    - logSongs: (song: string) => string은 getSongAt: (index: number) => string에 할당되도록 제공되는 타입이다.
        - logSongs의 song 매개변수는 getSongAt의 index 매개변수로 할당된다.
        - song의 string 타입은 index의 number 타입에 할당할 수 없다.
        


### 함수 타입 괄호

- 함수 타입은 다른 타입이 사용되는 모든 곳에 배치할 수 있다.
- 유니언 타입도 포함되며 애너테이션에서 함수 반환 위치를 나타내거나 유니언 타입을 감싸는 부분을 표시할때 괄호를 사용한다.

```ts
// 타입은  string | undefined 유니언을 변환하는 함수
let returnString:() => string | undefined;

// 타입은 undefined 나 string 을 반환하는 함수
let myReturn: (() => string) | undefined;
```


### 매개 변수 타입 추론
- 타입스크립트는 선언된 타입의 위치에 제공된 함수의 매개변수 타입을 유추 할 수있다
```ts
let singer: (songs: string) => string;
singer = function (song) {
  // song: string 타입
  return 'Singing: ${song.toUpperCase()}!'} // ok
```
- 함수를 매개변수로 갖는 함수에 인자로 전달 된 함수는 해당 매개변수 타입도 잘 유추 할 수 있다.
```ts
const songs = ["a", "b", "c];
               
// song: string
// index: number
songs.forEach((song, index) => {
  console.log('${song} is at index ${index}')   
});

```


### 함수 타입 별칭
- 함수 타입에서도 동일하게 타입 별칭을 사용할 수 있다.
- 함수 리터럴로 사용하면 코드 가독성이 안 좋다.
```ts
type StringNumber = (input: string) => number;

let stringNumber: StringNumber;

stringNumebr = (input) => input.length;  //ok

stringNumber = (input) => input.toUpperCase();
//Error: Type 'string' is not assignable to type 'number'
```
``` ts
type NumberString = (input: number) => string;

function useSNumberString(numberString: NumberString) {
  console.log('The string is: ${numberString(1234)}');
}

usesMunberString((input) => '${input}! Hooray!');  // ok

usesNumberString((input) => input * 2);

// Error: Type 'number' is not assignable to type 'string'
```


### 그 외 반환 타입
0️⃣ void 반환 타입
- void 는 어떤 값도 반환하지 않는다는 의미를 갖는 타입이다.
- return  문이 없는 함수이거나 값을 반환하지 않는 return 문을 가진 함수일 가진 함수는 어떤 값도 반환하지 않는다.
```ts
funtion logSong (song: string | undefined): void {
  if (!song) {
    return;   // ok
  }
  console.log('${song});
  
  return true;
  // Error : Type 'boolean' is not assignable to type 'void'
```
- 함수 타입 선언 시 void 반환 타입은 매우 유용하다.
- 함수 타입을 선언할 때 void를  사용하면 함수에서 반환되는 모든 값은 무시된다.
```ts
let songLogger: (song: string) => void;

songLogger = (song) => {
  console.log('${songs}');
};

songLogger("Fake Love");   // ok

```

- 자바스크립트 함수는 실젯값이 반환되지 않으면 기본으로 모두 undefined를 반환하지만 void는 undifined와 동일하지 않다.
- void는 함수의 반환 타입이 무시된다.
```ts
function returnVoid() {
  return;
}
let lazyValue: string | undefined;
lazyValue = returnVoid();
// Error : Type 'void' is not assignable to type 'string| undefined'.
```
- undefined와 void를 구분해서 사용하면 매우 유용하다.
- void 를 반환하도록 선언된 타입 위치에 전달된 함수가 반환된 모든 값을 무시하도록 설정할 때 유용하다.
- forEach 메서드는 void를 반환하는 콜백을 받아 forEach에 제공되는 함수는 원하는 모든 값을 반환할 수 있다.
- void 타입은 함수의 반환값이 자체적으로 반환될 수 있는 값도 아니고, 사용하기 위한 것도 아니라는 표시위한 것.

```ts
const records: string[] = [];

function saveRecords(newRecords: string[]) {
  newRecords.forEach(record => records.push(record));
}

saveRecords(['22', 'yet to come', 'permission to Dance'])

```


### never 반환 타입
- 일부 함수는 값을 반환하지 않고 never 반환 함수는 항상 오류를 발생시키거나 무한루프를 실행하는 함수이다. 
- 함수가 절대 반환하지 않도록 의도하려면 명시적 : never 타입 애너테이션을 추가해 해당 함수를 호출한 뒤 모든 코드가 실행되지 않음을 나타낸다. 
```ts
function fail(message: string): never {
  throw new Error ('Invariant failure: ${message}.');
}
function workWithUnsafeParam(param:unknown) {
  if(typeof param !== 'string') {
    fail('param should be a string, not ${typof param}');
  }
  
  param.toUpperCase();   // ok
}
```

### 🌧️ 함수 오버로드
- 선택적 매개변수나 나머지 매개변수만으로 표현할 수 없는 매우 댜른 매개변수들로 호출 될 수 있다.
- 하나의 최종 구현 시그니처와 그 함수와 본문 앞에 서로 다른 버전의 함수 이름, 매개변수, 변환 타입을 여러 번선언한다.
- 함수 오버로드는 복잡하고 설명하기 어려운 함수 타입에 사용하는 최후의 수단이기 때문에 가능하면 함수 오버로드를 사용하지 않는 것이 좋다.
```ts
function createDate(timestamp: number): Date;
function createDate(month: number, day: number, year: number): Date;
function createDate(monthOrTimestamp: number, day?:number, year?: number) {
  return day. === undefined || year === undefined
      ? new Date(monthOrTimestamp)
      : new Date (year, monthOrTimestamp, day);
}
createDate(1231);  // ok
createDate(2, 22, 2222);  // ok

createDate(4, 1);
// Error : No overload expects 2 arguments, but overloads
// do exist that expect eithor 1 or 3 arguments.
```



- createDate 함수 1개는 timestamp 매개변수 또는 3개의 매개변수 (month, day, year)를 사용해 호출한다.
- 2개의 인수를 사용해 호출하면 2개의 인수를 허용하는 오버로드 시그니처가 없기 때문에 타입 오류가 발생한다. 


### 호출 시그니처 호환성
- 함수의 오버로드 시그니처에 있는 반환 타입과 각 매개변수는 구현 시그니처에 있는 동일한 인덱스이 매개변수에 할당할 수 있어야 한다. (구현 시그니처는 모든 오버로드 시그니처와 호환되어야 함)

- 1. 처번째 매개변수를 string으로 선언
- 2. 두개의 오버로드 시그니처는 string 타입과 호환
- 3. 세번째 오버로드 시그니처의 () => string 타입과는 호환되지 않는다.


```ts
function format(data: string): string; // ok
function format(data: string, needle: string, haystack: string): string; // ok
function format(getData: () => string): string;
// Error: This overload signature is not compatible with its implementation signature

function format (data: string, neele? string, haystack? string) {
  return needle && haystack ? data. replace(needle, haystack) : data;
}
```

- 타입스크립트는 초기 배열에 데이터 타입이 있는지 기억하고, 배열이 해당 데이터 타입에서만 작동하도록 제한한다. (배열이 데이터 타입을 하나로 유지)
- 아래의 예제를 보면 string 타입 값 추가는 하용되지만 다른 데이터 타입 추가는 혀용되지 않는다.
```ts
const bestSongs = ["black swan", "I NEED YOU"];

// ok: "IDOL" 의 타입은 string
bestSongs.push("IDOL");

bestSongs.push(true);
// Error: Argument of type 'boolean' is not assignable to parameter of type 'string'

```


### 배열 타입
- 배열을 저장하기 위한 변수는 초깃값이 필요하지 않는다.
- 변수는 undefined 로 시작해서 나중에 배열 값을 받을 수 있다.
- 배열에 대한 타입 애너테이션은 배열의 요소 타입 다음에 []가 와야한다.
```ts

let btnAlbums: number[];
btnAlbums = [2, 4, 8, 12, 16, 18, 20, 22];
```


### 배열과 함수 타입
- 골호는 애너테이션의 어느 부분의 함수 반환 부분이고 어는 부분이고 어는 부분이 배열 타입 묶음인지 나타내기 위해 사용한다.
```ts
// 타입은 string 배열을 반환하는 함수
let arr1: () => string[];

// 타입은 각각의 string을 반환하는 함수 배열
let arr2: (() => string)[];
```


### 유니언 타입 배열
- 유니언 타입으로 베열 타입을 사용할 때 애너테이션의 어느 부분이 배열의 콘텐츠이고 어느 부분이 유니언 타입 묶은인지를 나태내기 위해 괄호를 사용해야 한다.
- 유니언 타입에서 괄호 사용은 매우 중요하다.
```ts
// 타입은 string 또는  number의 배열
let arr1: string | number[];

// 타입은 각각 number or string의 요소인 배열
let arr2: (string | number)[];
```


- string 값과 undefined 값을 모두 가지는 타입의 예
```ts
// 타입: (string | undefined)[]
const teamMember = [
  "taetae",
    "suga",
    "RM",
    undefined,
];
```

### any 배열의 진화
- 초기에 빈 배열로 설정된 변수에서 타입 애너테이션을 포함하지 않으면 타입스크립트는 배열을 arr[]로 취급하고 모든 콘텐츠를 받을 수있다.
- any 변수가 변경되는 것처럼 any[] 배열이 변경되는 것도 좋아하지 않는다.
- 타입 애너테이션이 없는 빈 배열은 잠재적으로 잘못된 값 추가를 허용해 타입스크립트의 타입 검사기가 갖는 이점을 부분적으로 무력화 한다.
```ts
// 타비 : any[]
let values: [];

// 타입 : string[]
values.push('');

// 타입: (number | string)[]
values[0] = 0;
```


### 다차원 배열
-2차원 배열 또는 배열의 배열은 두 개의 [](대괄호)를 갖는다.
```ts
let lifeGoesOn: number[][];

lifeGoesOn = [
  [1,2,3],
    [2,4,6],
    [3,6,9],
];
```

### 배열 멤버
- 배열의 멤버를 찾아서 해당 배열의 타입 요소를 되돌려주는 전형적인 인덱스 기반 접근방식이다.
```ts
const bestSongs = ["Run", "DNA"];

// 타입: string
const bestSong = bestSongs[0];
```

- 유니언 타입으로 된 배열의 멤버는 그 자체로 동일한 유니언 타입이다.
```ts
const btsMembers = ["V", new Date(1995, 12, 30)];

// 타입 : string | Date
const btsMember = btsMembers[0];
```


### 주의사항: 불안정한 멤버
- 타입스크립는 모든 배열의 멤버에 대한 접근이 해당 배열의 멤버를 반환한다고 가정하지만, 자바스크립트에서 조차도 배열의 길이보다 큰 인덱스로 배열 요소에 접근하면 undefined를 제공합니다.

```ts
function withMembers(members: string[]) {
    console.log(members[1995].length);  // 타입 오류 없음
}
withMembers(["For", "youth"]);
  
```

## 스프레드와 나머지 매개변수
### 스프레드
- ...스프레드 연산자를 사용해 배열을 결합합니다.
- 서로 다른 타입의 두 배열을 함께 스프레드해 새 배열을 생성하면 새 배열은 두 개의 원래 타입 중 어느 하나의 요소인 유니언 타입 배열로 이해된다.

```ts
// 타입: string[]
const btsMember = ["V", "JK", "Jimin"];

// 타입: number[]
const btsAge : [29, 27, 29];

// 타입 : (string | number)[]
const hybe = [...bts, ...btsAge];

```

### 나머지 매개변수 스프레드
- 타임스크립트는 나머지 매개변수로 배열을 스프레드하는 자바스크립트 실행을 인식하고 이에 대해 검사를 수행하낟. 
- 나머지 매개변수를 위한 인수로 사용되는 배열은 나머지 매개변수와 동일한 배열 타입을 가져야 한다.
```ts
function hybe(greeting: string, ...names: string[]) {
  for(const name of names) {
    console.log('${greeting}, ${name}!');
  }
}

const member = ["V", "RM", "JK"];
member("Hello", ...member);
const bithYears = [1995, 1994, 1997];
member("Born in", ...birthYears);
// Error: Argument of type 'number'  is not assignable to parameter of type 'string'.
```

### 튜플
- 고정된 크기의 배열을 튜플이라고한다.
- 배열의 모든 가능한 멤버를 갖는 유니언 타입보다 더 구체적읻.
- 튜플 타입을 선언하는 구문은 배열 리터럴처럼 보이지만 요소의 값 대신 타입을 적는다

```ts
let yearAndbts: [number, string];

yearAndbts = [7, "BangTan"];

yeearAndbts = [false, "Bangtan"];
// Error: Type 'boolean' is not assignable to type 'number'

yearAndbts = [7];
//Error: Type '[number]' is not assignable to type '[number, string]'.
// source has 1 elements(s) but target requires2 
```


- 단일 조건을 기반으로 두 개의 변수에 초깃값을 설정하는 것처럼 한 번에 여러 값을 할당하기 위해 튜플과 배열구조 분해 할당을 함께 자주 사용


```ts
// year 타입: number
// bts 타입 :  string
let [year, bts] = Math.random() > 0.5
  ? [7, "BangTan"]
  : [5, "NewJeans"];

```


### 튜플 할당 가능성
- 타입스크립트에서 튜플 타입은 가변길이이 배열 타입보다 더 구체적으로 처리된다.
- 가변길의 배열타입은 튜플 타입에 할당 할 수 없다.
```ts
// 타입: (boolean | number)[]
const pairLoose = [false, 123]


const pairTupleLoose: [boolean, number] = pairLoose;
// Error: Type '(number | boolean)[]' is not assignable to type '[boolean, number]'
// Target requires 2 elements(s) but source may have fewer.
```

- pairLoose 가 [boolean, number] 자체로 선언된 경우 pairTupleLoose에 대한 값 할당이 허용되었을 것이다.
- 타입스크립트는 튜플 타입의 튜플에 얼마나 많은 멤버가 있는지 알고 있기 때문에 길이가 다른 튜플은 서로 할당 할수 없다.

```ts

const bts = [boolean, number, string] = [false, 2013, "BangTan"];

const btsNewJeans: [boolean, number] = [bts[0], bts[1]];

const btsBlakPink: [boolean, number] = bts;
// Error: Type '[boolean, number, string]' is not assignable to type '[boolean, number]'
// source has 3 elements(s) but target allows only 2

```


### 나머지 매개변수로서의 튜플
- 튜플은 구체적인 길이와 요소타입정보를 가지는 배열로 간주되므로 함수에 전달할 인수를 저장하는데 특히 유용하다.
```ts
function newJeans(name: string, value: number) {
  console.log('${name} has ${value}');
}
const hybeArray = ["V", 1];
newJeans(..hybeArray);
// Error: A spread argument must either have a tuple type or be passed to a rest paramter.

const hybeTomorrow: [number, string] = [5, "TXT"];
newJeans(...hybeTomorrow);
// Error: Argument of type 'number is not assignable to parameter of type 'string'

const hybeSpring: [string, number] = ["flower", 4];
newJeans(hybeSpring);  // ok
```

- 나머지 매개변수 튜플을 사용하고 싶다면 여러번 함수를 호출하는 인수 목록을 배열에 저장해 함께 사용할 수 있다.
- 가변 길이의 배열 타입은 튜플 타입에 할당할 수 없다.

```ts
function logPair(name: string, value: [number, boolean]) {
  console.log('${name} has ${value[0]} (${value[1]}');
}

const trios: [string, [number, boolean]][] = [
  ["V", [1, true]],
    ["Rm", [2, false]],
    ["JK", [3, false]]
];

trios.forEach(trio => logtrio(..trio));  ///ok

trios.forEach(logpair);
//Error: Argument of type '(name: string, value: [number, boolean]) => vild
```

- 여러번 함수를 호출하는 인수 목록을 배열에 저정해 함께 사용할 수있다.
- trios는 튜플 배열이고, 각 튜플은 두 번째 멤버로 또 튜플을 가진다.
- `trios.forEach(trio => logTrio(...trio))`는 각  ...trio가 logTrio의 매개변수 타입과 일치하므로 안전하다. 
- `trio.forEach(logTrio)`는 첫번째 매개변수로 타입이 string인 [string, [number, boolean]] 전체를 전달하려고 시도하기 때문에 할당 할 수없다.


### 튜플 추론
-타입스크립트 생성된 배열을 튜플이 아닌 가변 길이의 배열로 취급한다.
- 배열이 변수의 초깃값 또는 함수에 대한 반환값으로 사용되고, 고정된 크기의 튜플이 아니라 유연한 크기의 배열로 가정한다.

```ts
//반환 타입 : (string | number)[]
function hybe(input: string) {
  return [input[0], input,.length];
}


// hybe 타입: string | number
// size 타입: string | number
const [hybeNew, size] = hybe("bts");
```
=> [string,  number] 아니라 (string | number)[]를 반환하는 것으로 유추 된다.

### 명시적 튜플 타입
- 튜플 타입도 타입 애너테이션에 사용 할 수 있다.
- 함수가 튜플 타입을 반환한다고 선언되고, 배열 리터럴을 반환한다면 해당 배열 리터럴을 일반적인 가변길이의 배열 대신 튜플로 간주된다.

```ts
//반환 타입 : [string, number]
function hybeToSizebts(input: string): [string,number] {
  return [inpit[0], input.length];
}

// hybeSize 타입: string
// size 타입 : number
const [hybeTo, size] = hybeNewJeans("bts");

```


### conset  어서션
- 명시적 타입 애너테이션에 튜플 타입을 입력하는 작얿은 명시적 타입 애너테이션을 입력할 때와 동일한 이유로 고통스러울 수 잇다 
- 코드 변경에 따라 작성 및 수정이 필요한 구문을 추가해야한다.
- 타입스크립트는 값 뒤에 넣을 수 있는 const 어서션인, as const 연산자를 제공
- 타입을 유추할 때 읽기 전용이 가능한 값 형식을 사용도록 지시함.
- as cosnt 가 배치되면 배열이 튜플로 처리되어야 한다. 

```ts
// 타입 : (string |number) 
const taetae = [1995, "V"];

// 타입 : readonly [1995, "V"]
const readonlyTuple = [1995, "V"] as const;
```

- const 어서션은 유연한 크기의 배열을 고정된 크기의 튜플로 전환하는 것을 넘어서, 해당 튜플이 읽기 전용이고 값 수정이 예상되는 곳에서 사용할 수없음을 나타낸다.

```ts
const taetae: [number, string] = [1995, "v"];
taeatae[0] = 12431; // ok

const taetaeNew: [number, string] = [1995, "v"] as const;

//Error: The type 'readonly [1995, "V"] is "readonly'
// and cannot be assigned to the mutable type '[number, string]'.

const taetaeConst = [1995, "v"] as const;
taetaeConst[0] = 1230;

// Error : Cannot asign to '0' because it is a read-only property.

```
알게된 점)
readonly 속성은 불리언 속성으로서 해당 속성을 명시하지 않으면 속성값이 자동으로 false 값을 가지게 되며, 명시되면 자동으로  true 값을 가지게 된다.

- 실제로 읽기 전용 튜플은 함수변환에 편리하다.
- 함수로부터 반환된 값은 보통즉시 구조화되지 않으므로 읽기 전용인 튜플은 함수를 사용하는데 방해가 되지 않는다.

```ts
// 반환 타입: readonly [string, number]
function hybeToSizeConst(input: string) {
  return [input[0], input.length] as const;
}

// hybeTo 타입 : string
// size 타입 : number
const [hybeTo, size] = hybetoSizeConst("V");
```

[Reference]
1. Learning typeScript(조시 골드버그/고승원/한빛미디어_2023)



























