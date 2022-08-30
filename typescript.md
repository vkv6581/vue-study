# Typescript

과정

이미 구현된 자바스크립트를 타입스크립트로 변환.

타입스크립트 기본 개념.

- 참고자료 : [타입스크립트 핸드북](https://joshua1988.github.io/ts/)

1. 타입스크립트
- 자바스크립트의 슈퍼셋(확장 언어)
- 자바스크립트에 타입이 부여된 언어.
- 실행을 위해 변환(컴파일)이 필요.


## 타입스크립트 장점
- 에러의 사전 방지 (컴파일 과정에서 에러 검출.)


javascript 기능으로 구현
```
/**
 * @typedef {object} summary
 * @property {string} id
 * @property {number} age
 */

/**
 * @returns {Promise<summary>}
 */
 ```

위와 같이 javascript에서 [jsDocs](https://devdocs.io/jsdoc/)로 타입을 지정할 수 있음.

자바스크립트의 타입 지정 가이드는 제공하지만 에러를 발생시키지는 않음.

타입스크립트로 넘어가게 되면 타입에 강제성이 생겨 없는 속성 사용 시 에러 발생함.


- 코드 자동완성(가이드)



## typescript 옵션 tsconfig.json

- compilerOptions.allowJs : 프로젝트 내 Js 허용
- compilerOptions.checkJs : js파일에서 타입 체크
- compilerOptions.noImplicitAny : 모든 변수에 타입 필수 지정

## Typescript 기본 타입
[핸드북 참고자료](https://joshua1988.github.io/ts/guide/basic-types.html#%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B8%B0%EB%B3%B8-%ED%83%80%EC%9E%85)

```
//TS 문자열
let var1: string = 'hello';

//TS 숫자
let var2: number = 10;

//TS 배열
let myArr: Array<number></number> = [1, 2, 3];  //숫자만 들어갈 수 있는 배열.

let myStrArr: Array<string> = ["test", "test2"] //문자열만 들어갈 수 있는 배열.

let items: number[] = [1,2,3];  //배열 리터럴을 통한 배열 선언.

// TS 튜플
let address: [string, number] = [
    'ljy', 29
];  //배열의 인덱스마다 타입이 정의되어 있는 타임.

//TS 객체
let obj: object = {};

let person: object = {
    name: 'ljy',
    age: 29
};

//객체 안의 변수들의 타입 지정.
let person: { name: string, age: number } = {
    name: 'ljy',
    age: 29
};

//TS boolean
let show: boolean = true;
```

기타 타입 (any, void)
- any : 모든 타입 가능 (타입 지정하지 않음)
- void : 함수의 반환값 없음.

any는 타입이 지정되지 않아 타입스크립트의 기능이 제한되므로 비추천.

## Typescript 함수
[핸드북 참고자료](https://joshua1988.github.io/ts/guide/functions.html)

```
//TS 함수 매개변수, 리턴값 타입 지정.
function sum(a: number, b: number): number {
    return a + b;
};

// TS 옵셔널 파라미터
//아래와 같이 함수 파라미터명 뒤에 ?를 붙이면 선택적으로 사용 가능.
function log(a: string, b?: string) {
    
};

log('hello');   //가능
log('hello', 'ljy') //가능
```

## Enum
- 열거형

```
enum Shoes {
    Nike,
    Adidas
}

var myShoses == Shoes.Nike;
console.log(myShoses);  //0 출력 (따로 선언하지 않으면 0부터 시작.)
```

- 문자열 열거형
```
enum Shoes {
    Nike = '나이키',
    Adidas = '아디다스'
}

var myShoses == Shoes.Nike;
console.log(myShoses);  //나이키 출력
```

- Enum 활용 사례
```
enum Answer {
    Yes = 'Y',
    No = 'N'
}

function askQuestion(answer: Answer) {
    if (answer === Answer.Yes) {
        TODO
    }
    ...
}
```

## 타입
- TODO : 추가 작성예정!

```
type Person = {
    name: string;
    age: number;
}
```

## 인터페이스
- Typescript에서 반복해서 사용할 타입 정의 (class처럼)

```
//TS 인터페이스 정의
interface User {
    age: number;
    name: string;
}

// 변수에 활용
let jy: User = {
    age: 29,
    name: '재영'
}
```

- 함수에 적용

(API의 리턴 형식 지정에 인터페이스를 가장 많이 사용.)
```
//함수에 활용 - 함수의 인자로 User형식만 받음.
function getUser(user: User) {
    console.log(user);
}

const ljyy = {
    name: '재영'
}

getUser(ljyy);   //에러 발생

const ljy2 = {
    age: 30,
    name: '재영2'
}

getUser(ljy2);   //ok
```

- 딕셔너리 패턴

```
interface StringArray{
    [index: number]: string;
}

var arr: StringArray = ['a', 'b', 'c'];

arr[0] = 10;    //에러 (number만 가능.)
```

```
//RegExp는 정규식 타입.
interface StringRegexDictionary {
    [key: string]: RegExp
}

//위의 인터페이스를 아래처럼 사용 가능
let obj: StringRegexDictionary = {
    cssFile: /\.css$/,
    jsFile: /\.js$/
}

//장점 - 아래와 같이 keys를 통해 사용 가능.
Object.keys(obj).forEach(function(value) {
    //TODO
});
```

- 인터페이스 확장(상속)
```
interface Person {
    name: string;
    age: number;
}

//Person을 상속한 Developer 선언
interface Developer extends Person {
    language: string;
}

//위의 Developer과 동일.
interface Developer {
    name: string;
    age: number;
    language: string;
}

```

## 타입 별칭(Type Aliases)
- 특정 타입이나 인터페이스를 참조할 수 있는 타입변수.

```
type MyName = string;
const name: MyName = 'leejaeyoung';
```

## 인터페이스와 타입의 차이
- 가장 큰 차이점 : 타입은 확장이 되지 않음.
- 사용법은 동일하나 자동완성 및 미리보기에서의 차이가 있음.

- 인터페이스

![인터페이스](https://joshua1988.github.io/ts/assets/img/interface-preview.c17462bf.png)


- 타입

![타입](https://joshua1988.github.io/ts/assets/img/type-preview.0035610f.png)

```
interface Person {
    name: string;
    age: number;
}

type PersonType {
    name: string;
    age: number;
}

//사용법은 동일
var jy: Person ={
    name: '재영',
    age": 29
}
```

## 유니온 타입(|)
- 하나의 변수에 여러 타입을 허용하게 함. (any와는 다름.)

```
//함수의 인자인 value에 string, number 두 개의 타입 둘 다 허용.
function logMessage(value: string | number) {
    console.log(value);
}

logMessage('hello');
logMessage(29);

//변수도 여러 타입 허용가능.
var jaeyoung: string | number | boolean;
```

- 유니온 타입의 장점
- any와 비슷하지만 타입별로 제약을 걸 수 있음. (타입 가드)

```
function logMessage(value: string | number) {
    if(typeof value === 'number') {
        todo...
    }
    else if(typeof value === 'string') {
        todo...
    }

    throw new TypeError("value invalid");
}
```

## 유니온 타입 제약사항

- typescript입장에선 여러 인터페이스의 항목이 넘어올 경우, 공통 속성만을 허용한다. (어떤 것이 넘어올지 컴파일러 입장에선 알 수 없음.)

- 타입 가드를 사용하여 타입별로 동작 분리를 시켜야 한다.

```
interface Person {
    name: string;
    age: number;
}

//Person을 상속한 Developer 선언
interface Developer extends Person {
    language: string;
}

function askSomeone(someone: Developer | Person) {
    someone.name;   //가능
    someone.language;  //오류 : 공통 속성만을 허용함.
    someone.age;    //가능
}
```

### 인터섹션 타입 (&)
- 여러 가지 인터페이스, 타입을 만족하는 변수를 선언할 때 사용.

```
//someone은 Developer 과 Person의 속성 전부를 가지게 된다.
function askSomeone(someone: Developer & Person) {
    someone.name;
    someone.language;
    someone.age;
}
```

### 클래스(javascript)
- ES6 문법
```
class Person {
    //클래스 로직

    //생성자
    constuctor() {
        this.name = name;
        this.age = age;
    }
}
var jy = new Person('재영', 29);
```

### 프로토타입(javascript)
```
var user = { name : '
ljy', age : 29};
var admin = {};
//admin에 user에 기본 정보들을 상속받는것처럼 사용 가능.
admin.__proto__ = user;

```

### 클래스(Typescript)
- javascipt의 클래스와 동일하지만 제약사항이 추가됨.
```
class Person {
    private name: string;
    public age: number;
    readonly log: string;

    //생성자
    constuctor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}
var jy = new Person('재영', 29);
```

### 제네릭(Generics)
- 타입을 함수의 파라미터처럼 받는 개념.
- 유니온 타입의 제약사항들을 해결할 수 있음 (함수중복, 리턴값의 타입 등.)

```
// 함수 호출 시점에 파라미터의 타입이 정해진다.
function logText<T>(text: T):T {
    console.log(text);
    return text;
}
logText<string>('하이요');  //파라미터의 타입이 문자열이 됨.
```

- 인터페이스에 제네릭을 선언
```
interface Dropdown {
    value: string;
    selected: boolean;
}

//value의 타입 유동적으로 변경 필요할 경우
interface Dropdown<T> {
    value: T;
    selected: boolean;
}

const obj = Dropdown<number> = {value: 10, selected: false}
```

- 제네릭에 타입 힌트
```
//제네릭에 들어올 타입이 배열이라고 힌트를 주는 것.
function logTextLength<T>(text: T[]): T[] {
    console.log(text.length);
}
```
```
interface ShoppingItem {
    name: string;
    price: number;
    stock: number;
}

function getShoppingItemOption<T extends keyof ShoppingItem>(itemOptin: T): T {
    return itemOption;
}

## 타입 추론
- 타입스크립트에서 타입을 추론하는 방법.
- vscode의 인텔리센스, 타입스크립트의 코드 서버를 통해 실시간으로 체크가 이루어짐.

## 용어 설명
- 타이핑(Typing) : 변수들에 타입을 맵핑하는 것.