# 1. TDD

## 1. TDD

> 💡 테스트 코드를 먼저 작성하는, 즉 구현보다 인터페이스와 스펙을 먼저 정의함으로써 개발을 진행하는 방식

[테스트 주도 개발](https://github.com/ahastudio/til/blob/main/agile/test-driven-development.md)  
[TDD FAQ](https://github.com/ahastudio/til/blob/main/blog/2016/12-03-tdd-faq.md)  
[Jest를 이용한 간단한 TDD 예제](https://github.com/ahastudio/til/blob/main/jest/20201204-simple-tdd-example.md)

### TDD Cycle

> 📌 다음 과정을 반복하며 일정 수준에 도달할 때까지 진행

1. 🔴 Red → 실패하는 테스트 코드를 작성. 인터페이스와 스펙에 집중
   1. 인터페이스 : 시그니처의 집합
   2. 시그니처 : Ex. `add(x, y) -> number`
   3. 스펙 : 어떤 예제의 기댓값 
2. 🟢 Green → 재빨리 테스트를 통과시키기. 올바른 방법이 아니어도 괜찮음
   1. 가짜로 작성하기([Mocking](https://ko.wikipedia.org/wiki/%EB%AA%A8%EC%9D%98_%EA%B0%9D%EC%B2%B4))
   2. [brute-force](https://en.wikipedia.org/wiki/Brute-force_search)
   3. 죄악 저지르기
3. 🛠 Refactor → 리팩토링을 통해 코드를 올바르게 만들기

* 사이클 소요 시간 : 1분 이하가 최고, 10분 안에 한 사이클을 돌릴 수 있어야 함
  * 실패하는 테스트를 가짜로라도 작성해야 하는데 너무 오래 걸린다면, 1번이 너무 큰 것
  * **더 쉽고 작은 문제를 정의해야 함**

> 🙋🏻‍♀️ **주의 사항**
>
> 작은 단계를 찾고, 코드에서 피드백을 얻는 것이 어렵고 중요한 포인트      
> 2번이 어렵다면 1번으로 돌아가서 더 작고 쉬운 문제를 정의하고,  
> 3번을 위해 의도를 드러내고 중복을 찾아 제거하는 연습을 해야 함     
> 이 둘이 익숙하지 않으면 TDD를 하는 게 거의 불가능하고, 이 둘이 어려우면 일반적인 개발 또는 클린 코드를 작성하는 것 또한 매우 힘듦  



### 리팩토링 

> 📌 TDD에서 가장 중요한 부분이지만, 간과될 때가 많음

* TDD의 목표는 제대로 작동하는 클린코드 → 리팩토링 덕분에 가능해짐 
* 단위테스트를 통해 테스트가 터지지 않는 선에서 계속 리팩토링
* 동작은 바뀌지 않고 설계만 바뀌는 것(스펙은 그대로)

### 중복 제거

안 보이면 연습해야 함  
의도를 드러내면 중복이 보이는 경우도 있음  
중복을 계속 만들어낸 후 패턴을 찾아내서 정리

> ⚠️ **TDD에 대해서 주의할 점**
>
> 테스트 코드를 작성한다고 해서 TDD 가 아닙니다.
> TDD Cycle 에 따라 테스트 코드를 먼저 작성하고, 구현하고, 리팩터링 하는 과정을 엄격하게 지켜서 개발을 진행해야 TDD 라고 할 수 있습니다.
> TDD 는 테스트 코드를 작성하는 것과 별개로 따로 연습이 필요하고 습관을 들여야하는 분야입니다.
> 그리고 TDD 를 잘 하기 위해서는 테스트 코드 작성법 자체부터 공부를 하셔야 합니다.
> 당장은 TDD 보다 테스트 코드 작성에 익숙해지고 어떻게 하면 테스트 코드를 잘 작성할 수 있을까에 초점을 맞춰 공부하시면 좋습니다.

<br>

## 2. Jest

[Jest](https://jestjs.io/)    
[Given-When-Then](https://github.com/ahastudio/til/blob/main/blog/2018/12-08-given-when-then.md)    

Jest는 단순성에 초점을 둔 JavaScript 테스트 프레임워크  
Jest는 Mocha와 Chai처럼 RSpec의 describe-it을 지원하고, expect로 단언(assertion) 가능  
Mocking도 다양한 레벨에서 쉽게 사용할 수 있음  
⇒ 거의 모든 것을 갖춘 테스팅 도구 🛠  

### 파일명

```
🏷 테스트하고 싶은 것.spec | test.확장자
```

* `sample.test.ts`   
* `sample.spec.ts` - 주로 BDD 스타일로 사용할 때  

#### npx jest를 실행했을 때 터미널에 출력되는 내용

```
 testMatch: **/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x) - 0 matches
```

### 테스트 케이스

테스트 해야하는 하나 하나의 케이스

#### 테스트 케이스를 정의하는 두 가지 방법

1. test 함수로 개별 테스트를 나열하는 방식
2. BDD 스타일로 `주체-행위 중심`으로 테스트를 조직화하는 방식

### jest.config.js

Jest에서 TypeScript 사용하도록 `jest.config.js` 파일 작성

```js
module.exports = {
    testEnvironment: 'jsdom', 
   setupFilesAfterEnv: [
       '@testing-library/jest-dom/extend-expect',
   ], 
   transform: {
        '^.+\\.(t|j)sx?$': ['@swc/jest', {
            jsc: {
                parser: {
                    syntax: 'typescript', 
                   jsx: true, 
                   decorators: true,
                }, 
               transform: {
                    react: {
                        runtime: 'automatic',
                    },
               },
            },
        }],
   },
};
```

### Jest 실행하기

```bash
# jest 실행
npx jest

# jest 자동 실행
npx jest --watchAll
# 이후 Enter 입력
```

`npx jest --watchAll`은 저장하거나 내용이 바뀔 때마다 자동으로 테스트를 진행

> 🚨 **외부 의존성**
> 
> 서버를 띄우지 않고 테스트를 실행하면 오류 발생  

### SWC

[SWC 공식문서](https://swc.rs/docs/getting-started)  

Rust로 만든 타입스크립트 컴파일러

Jest는 타입스크립트 검증을 하지 않음 → SWC가 타입을 빼고 테스트를 돌리기 때문  
**타입을 잡고 싶다면** 다음 명령어를 실행하면 됨 

```
npx tsc --noEmit
```

컴파일을 진행한 결과가 js 파일로 생기는데, 이것을 생기지 않도록 하는 명령  

<br>

## 3. 테스트 코드 작성하기 

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/323af36920ec5bacfcb7f0c49339c817ec7c8a3e)

### test 함수로 개별 테스트 작성

처음에는 이러한 방식으로 진행해볼 것 

```js
function add(x: number, y: number): number {
   return 1 + 2;
}

test('add', () => {
    expect(add(1, 2)).toBe(3);
});
```

```js
function add(x: number, y: number): number {
   return x + y; // 중복을 제거하면서 의도를 명확히 드러내기
}

test('add', () => {
    expect(add(1, 2)).toBe(3);
});
```

### BDD 스타일로 테스트 대상과 행위를 명확히 드러내기

> 📝 **BDD**
>
> * 어떤 것을 테스트 할 것이다
> * 어떻게 될 것이다
> 
> 💡 단순한 경우에는 test 함수로 간단하게 작성, BDD 스타일은 복잡한 테스트를 진행할 경우에 유용  

```js
describe('add', () => {
    it('returns sum of two numbers', () => {
        expect(add(1, 2)).toBe(3);
    });
});
```

* `add` - 주어
* `it` - 주어는 ~ 하다
* `expect` - 기존의 경우와 동일, 기대값

### 다양한 경우 고려하기(Describe - Context - It 패턴)

```js
const context = describe;

describe('add', () => {
    context('with no argument', () => {
        it('returns zero', () => {
            expect(add()).toBe(0);
        });
    });
    
    context('with only one number', () => {
        it('returns the same number', () => {
            expect(add(1)).toBe(1);
        });
    });
    
    context('with two numbers', () => {
        it('returns sum of two numbers', () => {
            expect(add(1, 2)).toBe(3);
        });
    });
    
    context('with three numbers', () => {
        it('returns sum of three numbers', () => {
            expect(add(1, 2, 3)).toBe(6);
        });
    });
});
```

이 과정에서 add 함수가 바뀌는데, 이런 방법이 TDD

<br>

## 4. Describe - Context - It 패턴

상황마다 다른 맥락을 묘사하는 것  
코드의 행동을 설명하는 패턴이기 때문에 이해하기 쉬움  

RSpec의 describe-it에서 따온 패턴    
`when` 또는 `with` 사용  
context를 지원하지 않음

```js
// 해결 방법
const context = describe;
```

### 장점

* 쓰기 편하고 코드의 의도가 명확해짐
* 다양한 상황을 고려할 수 있음 → 테스트 커버리지의 증대
* 전역변수를 남발하는 문제 해결 


### 중복 제거 예시 

> ❓ **어디까지 리팩토링 해야 할까?**
> 
> 내가 고치고 싶은 만큼

#### 처음 시작 함수

```js
function add(x: number, y: number): number { 
    return x + y; 
}
```

#### 다양한 경우를 고려하며 변형된 함수  

```js
function add(...numbers: number[]): number {
    return (numbers[0] ?? 0) + (numbers[1] ?? 0) + (numbers[2] ?? 0);
}
```

#### 패턴 파악하기  

```js
function add(...numbers: number[]): number {
    return (numbers[numbers.length - 3] ?? 0) 
            + (numbers[numbers.length - 2] ?? 0) 
            + (numbers[numbers.length - 1] ?? 0);
}
```

```js
function add(...numbers: number[]): number {
    if (numbers.length === 0) {
        return 0;
    }
    if (numbers.length === 1) {
        return numbers[0];
    }
    if (numbers.length === 2) {
        return numbers[0] + numbers[1];
    }
    if (numbers.length === 3) {
        return numbers[0] + numbers[1] + numbers[2];
    }
}
```

#### 재귀를 통한 풀이 

아랫줄부터 거슬러 올라기는 방식으로 해결 

```js
   if (numbers.length === 3) {
         return add(...numbers.slice(0, numbers.length - 1)) 
                 + numbers[numbers.length - 1];
}
```

```js
    if (numbers.length >= 2) {
        return add(...numbers.slice(0, numbers.length - 1)) 
                + numbers[numbers.length - 1];
}
```

```js
    if (numbers.length >= 1) {
        return add(...numbers.slice(0, numbers.length - 1)) 
                + numbers[numbers.length - 1];
}
```

```js
function add(...numbers: number[]): number {
    if (numbers.length === 0) {
        return 0;
    }
    
    return add(...numbers.slice(0, numbers.length - 1)) 
            + numbers[numbers.length - 1];
}
```

#### 최종 코드

```js
function add(...numbers: number[]): number {
    if (numbers.length === 0) {
        return 0;
    }
    
    return numbers.reduce((acc, number) => acc + number);
}
```

재귀를 사용하는 방법은 reduce를 사용하면 거의 다 해결 가능   


<br>

## 5. 단위 테스트

[유닛 테스트](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%9B_%ED%85%8C%EC%8A%A4%ED%8A%B8)

소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차  
모든 함수와 메소드에 대한 테스트 케이스(Test case)를 작성하는 절차  
