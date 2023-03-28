(첫주라 책만으로는 감이 안잡혀서 실습한 문제 위주로 정리했습니다)

```ts
let 이름: string = "yeom";
```

변수명 : 타입명

타입으로 쓸수있는것들> string, number, boolean, bigint, null, undefined,[], {}

```ts
let 이름: string[] = ["yeom", "mango"];
let 나이: { age: number } = { age: number };
```

array나 객체는 위와같이 타입지정 가능

#### 연습

```ts
let name: string[] = ["yeom", "mango"];
let old: { age: number } = { age: number };
```

또한 타입은 연산자를 통해 표현할 수 있다.

```ts
let 이름: string | number = "yeom";
```

->type이 string이나 number가 오게된다.

---

```ts
type nameType = string | number;
let 이름: nameType = "yeom";
```

-> type 키워드를 이용해서 타입을 변수처럼 담아서 사용하는것도 가능

---

```ts
type NameType = "yeom" | "mango";
let 이름: NameType = "yeom";
```

이런식으로 작성하면 이름이라는 변수에는 'yeom'이랑 'mango'만 들어올 수 있음
literal type이라고 부른다.

---

```ts
function 함수명(x: number): number {
  return x * 2;
}
```

함수는 파라미터와 return 값을 지정할 수 있음
다른 타입이 들어올경우 error
함수는 return 타입으로 void를 설정가능한데 return이 없는지 체크할 수 있는 타입

밑으로는 예제

```ts
//에러
function 함수명(x: number | string) {
  return x * 2;
}

//가능
function 함수명(x: number | string) {
  if (typeof x === "number") {
    return x * 2;
  }
}
```

가능한 예제처럼 x의 type을 확실히 지정해줘야한다.
변수의 타입이 확실하지 않으면 마음대로 연산불가능!

항상 타입이 무엇인지 미리 체크하는 narrowing 또는 assertion 문법을 사용해야 허락해준다.

---

array 자료 안에 순서를 포함해서 어떤 자료가 들어올건지 정확히 지정하고 싶으면 tuple 타입을 쓰면된다.

```ts
type Member = [number, boolean];
let yeomdogyeong: Member = [100, true];
```

---

object 타입도 정의가 너무 길면 type 변수에 담아 사용가능

```ts
type MyObject = {
  name?: string; //?물음표는 name속성이 선택사항이라는뜻
  age: number;
};

let 도경: MyObject = {
  name: "yeom",
  age: 5,
};
```

---

object안에 무슨 속성이 들어갈지 모른다?!
그렇다면
모두~ 전부 다~ 타입지정도 가능 index signature이라고함

```ts
type MyObject = {
  [key: string]: number;
};

let mango: MyObject = {
  age: 5,
  weight: 4,
};
```

---

class 타입 설정

```ts
class Person {
  name;
  constructor(name: string) {
    this.name = name;
  }
}
```

위에 보면 중괄호 내에 미리 name 변수를 만든것처럼 변수를 만들어놔야 constructor 안에서 this.name 처럼 사용가능

---

## ts 부가사항 ( 설치관련 )

typescripts는 코드 에디터 부가기능 느낌

#### 일반 html css js 웹개발시 타입스크립트 사용하려면

> npm install typescript

어쩌구.ts 파일 생성하면 ts 파일임

---

tsconfig.json 생성해서

```
{
  "compilerOptions" : {
    "target": "es5",
    "module": "commonjs",
  }
}
```

이거 넣어준다.

TS파일은 JS파일이랑 똑같이 사용가능하지만
웹브라우저는 TS 파일을 알아듣지 못하기 때문에 JS 파일로 변환 작업을 해야한다.

JS파일로 변환하려면 에디터에서 TERMINAL을 새로 연다음 tsc -w 입력해두면 얘가 자동으로 ts파일을 js파일로 근처에 변환해줌.

html파일 등에서 타입스크립트를 작성한 코드를 사용하려면

> `<script src="변환된파일.js"></script>`

변환된 .js 파일 사용할것

---

#### react프로젝트에서 type script 사용할경우

이미 있는 react project에서 터미널 켜고

> npm install --save typescript @types/node @types/react @types/react-dom @types/jest

아니면 처음부터 typescript 적용된 react 프로젝트 생성하려면

> npx create-react-app my-app --template typescript

---
