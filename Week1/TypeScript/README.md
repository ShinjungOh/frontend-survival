# 2. TypeScript

## 1. REPL

REPL(read-eval-print loop)   
CLI, 콘솔 환경, 인터랙티브 톱레벨(interactive toplevel), 랭기지 셸(language shell) 등으로도 불림  
단일 사용자의 입력을 받아 이를 평가(실행)하고 결과를 사용자에게 반환하는 컴퓨터 프로그래밍 환경  


### ts-node 

[npm ts-node](https://www.npmjs.com/package/ts-node)

ts-node는 TypeScript 실행기이면서 Node.js용 REPL  
타입스크립트는 컴파일 언어이기 때문에 자바스크립트로 변환이 필요  
💡 ts-node는 타입스크립트를 자바스크립트로 변환해주기 때문에 **컴파일 없이도** Node.js에서 **타입스크립트를 직접 실행**할 수 있음  
💡 개발 환경에서 타입스크립트 코드를 테스트할 수 있음 

```
// REPL 실행
npx ts-node


// 종료
Ctrl + C or .exit
```

* 개발 환경에서만 이용할 것 
  * `npm install -D typescript`
  * `npm install -D ts-node`

<br>

## 2. TypeScript

[typescrip 공식문서](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html)

`JS + TS 문법 (syntax)`    
타입스크립트는 자바스크립트에 타입문법을 추가해서 만든 슈퍼셋으로, 타입 개념을 js에 도입한 것     
정적 타입의 특성을 가지고 있고, 최종적으로 자바스크립트로 컴파일됨  
💡 컴파일이란, `번역`의 개념  


### Interface vs Type

[타입 별칭과 인터페이스의 차이점](https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD%EA%B3%BC-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

거의 비슷하고, 대체로 선호에 따라 선택  
타입 확장 방법 등에서 차이가 존재 (인터페이스 - 확장 가능, 타입 - 확장 불가능)

### Interface 

인터페이스는 객체처럼 `{ }` 로 선언   
extends로 확장해서 사용할 수 있음  

```ts
interface Person {
    name: string;
    age: number;
};
```

### Type

타입을 잡아주는 것   
타입은 변수처럼 `=` 을 사용해 할당하며, 필요한 것을 다 집어넣을 수 있음 

```ts
type Human = {
  name: string;
  age: number;
};
```

<br>

### 타입 추론

[타입 추론](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html#%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-types-by-inference)

타입을 일일이 잡아주기 힘들 때, 모든 타입을 다 적어주지 않더라도 알아서 타입을 추론해줌

> 💡 타입스크립트와 VS Code 모두 **마이크로소프트**가 개발  
> 마이크로소프트가 VS Code라는 IDE를 직접 만든 이유는 **타입스크립트를 전파**하기 위함    
> VS Code는 타입스크립트와 일렉트론으로 만들었기 때문에, 타입스크립트는 IDE와 통합되어 **자동 완성**이 지원되고 **오류를 캐치**하기 쉬움   
> 타입스크립트가 JS와 높은 호환성을 가지는 이유는 유저가 사용하고 있는 서비스를 계속 사용할 수 있게 하기 위함
> 
> 📅 **타입스크립트** : 2012년 10월 1일  
> 📅 **VS Code** : 2015년 4월 29일

<br>

### Union Type vs Intersection Type

### Union Type

합집합  
`여러 타입 중 하나`일 수 있음을 선언하는 방법  

```ts
type MyBool = true | false;
```

#### 매개변수를 제한할 때 유용

```ts
type Category = 'food' | 'toy' | 'bag';

function fetchProducts({ category }: { category: Category }) {
  console.log(`Fetch ${category}`);
}
```

#### 레거시 환경 또는 코드에서 사용

원래 자바스크립트에는 타입이 없었기 때문  
대표적으로 [ReactNode](https://github.com/facebook/react/blob/main/packages/shared/ReactTypes.js) 

```ts
export type ReactNode =
  | React$Element<any>
  | ReactPortal
  | ReactText
  | ReactFragment
  | ReactProvider<any>
  | ReactConsumer<any>;
```

### Intersection Type

교집합   
`타입을 확장`하는 방법, 여러 속성을 가짐

```ts
type Human = {
  name: string;
  age: number;
};

type Creature = {
  hp: number;
  mp: number;
};

type Person = Human & Creature;

let person: Person;

person = { name: '홍길동', age: 13, hp: 256, mp: 16 };
```

<br>

### Optional Parameter

[Optional Parameter](https://www.typescriptlang.org/docs/handbook/2/functions.html#optional-parameters)

함수에 매개변수를 넣지 않으면 undefined가 나옴  
타입을 사용하면 무조건 매개변수를 넣도록 에러가 발생  
매개변수를 넣지 않고도 사용할 수 있도록 Optional Parameter를 사용  
`물음표 기호(?)`

#### 사용 방법 

```ts
// Optional Parameter 사용 
function greeting(name?: string): string {
  return `Hello, ${name || 'world'}`;
}


// 기본값 설정 
function greeting(name: string = 'world'): string {
    return `Hello, ${name}`;
}
```

#### 매개변수가 오브젝트일 때 많이 활용

```ts
function greeting({ name, age }: {
  name: string;
  age?: number;  
}): string {
  return age ? `${name} (${age})` : name;
}
```

* ⚠️ ts-node에서는 한 줄로 붙여 쓰거나, type 등으로 따로 분리해야 해석 가능  
