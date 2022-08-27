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

## 용어 설명
- 타이핑(Typing) : 변수들에 타입을 맵핑하는 것.