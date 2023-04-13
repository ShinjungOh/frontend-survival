# 2. TSyringe

## 1. TSyringe

[TSyringe](https://github.com/microsoft/tsyringe)  

마이크로소프트에서 만든 TypeScript용 DI 도구(IoC Container)

> <em>Lightweight dependency injection container for JavaScript/TypeScript</em>  
가벼운 JavaScript/TypeScript용 종속성 주입 컨테이너

> 💉 **Syringe** 
> 주사기 

### 특징

* External Store를 관리하는데 활용할 수 있음
* React 컴포넌트 입장에서는 **전역**처럼 쓸 수도 있음
* **Props Drilling** 문제를 해결할 수 있는 방법 중 하나 (React로 한정하면 Context도 사용 가능)

### Props Drilling

[The problem with passing props](https://react.dev/learn/passing-data-deeply-with-context#the-problem-with-passing-props)

React 애플리케이션에서 데이터는 위에서 아래로(👨‍👩‍부모 -> 👧자식) props를 통해 전달  
props를 애플리케이션 안의 여러 컴포넌트에 전해줘야하는 경우에 발생하는 문제 

1. 너무 많은 컴포넌트 단계를 거쳐야 함 
2. 중간 컴포넌트가 props를 이용하지 않아도 넘겨줘야 함 
3. 형제 관계의 컴포넌트 간의 props 전달이 복잡

⇒ ⚠️ 데이터가 많아지고, 컴포넌트를 더 많이 거쳐야 하는 경우 **유지 보수성이 낮아질 수 있음**

> 🚧 **Context API**
> 
> React에서 제공하는 내장 API  
> Context를 사용해서 프롭스 드릴링 문제를 해결할 수도 있지만, 
> Context는 전체를 바꾸기 때문에 비효율적인 측면이 존재   
> Redux도 Context로 스토어를 내려줌  
> context 값이 변경되면 전체가 강제로 리렌더링 발생 

### 사용 방법 

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/fbab3a930b92ddbf760b39a7c7bc1127d9ee4b90)

#### 1. 의존성 설치

```
npm i tsyringe reflect-metadata
```

(참고) reflect-metadata가 아닌 다른 것을 설치해도 됨

> Add a polyfill for the Reflect API (examples below use reflect-metadata). You can use:
> 
> * [reflect-metadata](https://www.npmjs.com/package/reflect-metadata) 
> * [core-js (core-js/es7/reflect)](https://www.npmjs.com/package/core-js) 
> * [reflection](https://www.npmjs.com/package/@abraham/reflection)

#### 2. reflect-metadata import

`src/main.tsx` 파일과 `src/setupTests.ts` 파일에서 reflect-metadata를 import
* 모든 것이 시작하는 곳에 import 

```ts
import 'reflect-metadata';
```

> import 하지 않을 경우 에러 발생  
> 
> 🚨 <em>Error: tsyringe requires a reflect polyfill. Please add 'import "reflect-metadata"' to the top of your entry point.</em>

> `jest.config.js`에 설정 추가
> 
> ```js
> setupFilesAfterEnv: [
>   '@testing-library/jest-dom/extend-expect', 
>   '<rootDir>/src/setupTests.ts',	
> ]
> ```

#### 3. `tsconfig.json` 파일에 decorator 설정 주석 해제

```
"experimentalDecorators": true,    
"emitDecoratorMetadata": true,  
```

decorator : @사용하기

#### 4. Store 클래스 생성 

싱글톤으로 관리할 CounterStore 클래스를 준비

```ts
import { singleton } from 'tsyringe';

@singleton()
export default class CounterStore {
    // …(중략)...
}
```

#### 5. 싱글톤 Store 객체 사용

싱글톤 CounterStore 객체를 사용

```ts
import { container } from 'tsyringe';

const counterStore = container.resolve(CounterStore);
```

* 전역에서 하나만 존재
* IoC Container가 객체 생성을 알아서 해줌
* 알아서 조립해주는 factory 기능 탑재  

#### 6. 테스트에서 초기화하기

테스트에서 TSyringe에서 관리하는 객체를 초기화할 수 있음  

```ts
beforeEach(() => {
    container.clearInstances();
});
```

### 상태 변경 알림

Store는 어떤 식으로든 action을 처리하고, 상태가 바뀌면 연결된 컴포넌트를 forceUpdate  
컴포넌트는 해당 Store에서 상태를 얻어서 UI를 업데이트하게 되는데, 선언형 UI가 얼마나 편한지 절실히 느낄 수 있는 포인트

<br>

## 2. 의존성 주입(Dependency Injection)

[의존성 주입](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85)

뭔가를 쓰고 싶을 때, 전역변수를 통해 얻는 것이 아니라 제대로된 방법으로 얻어서 사용하고 싶을 때 사용  
의존성 주입(dependency injection)은 하나의 객체가 다른 객체의 의존성을 제공하는 테크닉  

### 장점

* 모듈 간 결합을 느슨하게 만듦
* 객체의 생성과 사용의 **관심사 분리**를 통해 가독성과 코드 재사용을 높임 
  * 각각이 어떻게 구현되어 있는지 알지 못해도 됨 
* 의존관계 역전 원칙과 단일 책임 원칙을 따르도록 클라이언트의 생성에 대한 의존성을 사용자의 행위로부터 분리
* 모의 객체(모킹) 등을 이용한 단위 테스트의 편의성을 높여줌

<br>

## 3. reflect-metadata

[reflect-metadata](https://github.com/rbuckton/reflect-metadata)  
[Introduction to “reflect-metadata” package](https://medium.com/jspoint/introduction-to-reflect-metadata-package-and-its-ecmascript-proposal-8798405d7d88)

TSyringe가 내부적으로 사용하는 것   
데코레이터를 디자인하기 위해 TypeScript에서 사용하는 패키지  
reflect-metadata를 사용하면 클래스, 클래스 필드 등에 사용자 지정 메타데이터를 추가할 수 있음

* 메타데이터 : 실제 데이터에 대한 추가 정보

### TypeScript Decorators

[Decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)

TypeScript 및 ES6에 클래스가 도입되면서, 클래스 및 클래스 멤버에 주석을 달거나 수정하는 것을 지원하기 위해 추가 기능이 필요해짐   
데코레이터는 `클래스 선언` 및 `멤버에 대한 주석과 메타 프로그래밍 구문을 모두 추가`하는 방법을 제공   
데코레이터는 JavaScript의 2단계 제안이자, TypeScript의 실험적 기능

```ts
import { singleton } from 'tsyringe';

@singleton()
export default class CounterStore {
  // 내용
}
```

<br>

## 4. Singleton (싱글톤)

[Singleton 패턴](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)

하나의 클래스에 하나의 인스턴스만 가지는 패턴  
전역에서 하나

### 장점

* 인스턴스 생성에 장점
* 하나의 인스턴스를 기반으로 해당 인스턴스를 다른 모듈들이 공유해서 사용하기 때문에 인스턴스 생성 비용이 줄어듦
* **I/O 바운드** 작업에 많이 사용
    * 네트워크, 데이터베이스, 파일 시스템에 요청하는 것
    * 계산을 완료하는 데 걸리는 시간이 주로 입력/출력 작업이 완료될 때까지 대기한 기간에 의해 결정되는 조건
    * 시간이 오래 소요
    * 인스턴스 생성 비용이 많이듦

### 단점

* 의존성이 높아짐
* **TDD**에 단점 (Test Driven Development 테스트 주도 개발)
    * TDD는 단위 테스트를 주로 함
    * 단위 테스트는 테스트가 서로 독립적이어야 함
    * 테스트를 어떤 순서로든 실행할 수 있어야 함
    * 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이기 때문에, 각 테스트마다 독립적인 인스턴스를 만들기 어려움
