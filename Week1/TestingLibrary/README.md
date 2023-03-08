# 4. Testing Library

## 1. Jest

[Jest 공식문서](https://jestjs.io/)  
Jest는 단순성에 초점을 둔 JavaScript 테스트 프레임워크  

Jest는 Mocha와 Chai처럼 RSpec의 **describe-it**을 지원하고, **expect로 단언(assertion)** 가능    
**Mocking**도 다양한 레벨에서 쉽게 사용할 수 있음   
⇒ 거의 모든 것을 갖춘 테스팅 도구 🛠

### (참고) 다양한 테스트 도구 

#### **Mocha**   

[Mocha](https://mochajs.org/)  
Node.js 와 브라우저에서 실행되는 기능이 풍부한 JavaScript 테스트 프레임워크  

#### **Chai**  

Mocha를 사용할 때 쓰는 어설션 라이브러리        
`expect(), assert(), should-style assertions`  

#### **RSpec**  

[RSpec](https://rspec.info/)  
Ruby용 테스트 도구  
<em>TDD를 생산적이고 재미있게 만들기</em>가 소개 문구   
원조  

- [BETTER SPECS](https://www.betterspecs.org/) 
  - RSpec 베스트 프랙티스 모음 
  - 이대로 사용할 수는 없지만 참고용, 흐름을 알 수 있음
  - Jest는 RSpec의 let 등을 지원하지 않기 때문에, 핵심 아이디어만 참고  

<br><br>

## 2. 테스트

🐞 버그를 찾는 행위로 생각하기 쉬움  
버그는 기대하지 않은(unexpected) 일이 발생하는 것 → `스펙, 사양(specification)을 명확히` 하는 것이 매우 중요  
💡 **스펙에 맞게 작동하는지를 확인하는 것이 테스트**

#### 기본적인 테스트 코드

```jsx
test('add', () => {
  expect(add(1, 2)).toBe(3);
});
```

### 최초의 버그

> **최초의 버그**   
> [그레이스 호퍼 (Grace Hopper)](https://en.wikipedia.org/wiki/Grace_Hopper)  
> 
> 최초 여성 프로그래머 중 한 명  
> 최초의 버그 발견 - 진공관의 나방  
> 컴파일러 개념을 만듦    
> 초기 고급 프로그래밍 언어인 COBOL을 개발     

<br>

### TDD

테스트 주도 개발, Test-Driven Development  
테스트를 먼저 작성하고 그 후 코드를 작성  

#### TDD의 순서 

> 🔴 Red(Fail) → 🟢 Green(Psss) → 🛠 Refactor

1. 실패하는 테스트 작성
2. 빨리 통과하도록 작성
3. 리팩토링으로 코드 개선 

<br>

### BDD

행위 주도 개발, Behavior-Driven Development  
TDD의 한 갈래

기능(feature) 목록을 만들고 구현하면 됨(행위를 먼저 작성)    
즉, 소프트웨어의 동작(Behavior)에 대한 것

#### Give/When/Then 패턴

모든 BDD 시나리오에 있는 3가지 핵심 요소

* GIVEN (context, 문맥 설명)
* WHEN (action, 동작 설명)
* THEN (outcome, 결과 설명)

#### BDD 스타일의 테스트 코드

```jsx
describe('add', () => {
  it('returns sum of two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```

케이스를 나눠 코드를 짜기 때문에 표현력이 좋아지고, 다양한 상황에 대해 고민할 수 있음

[(참고) Writing BDD Test Scenarios](https://www.departmentofproduct.com/blog/writing-bdd-test-scenarios/)  
[(참고) Describe-Context-It](https://velog.io/@zzezze/TIL-Describe-Context-It)

<br>

## 2. Describe-Context-It 패턴

상황마다 다른 맥락을 묘사하는 것  
코드의 행동을 설명하는 패턴이기 때문에 이해하기 쉬움  

### 장점

1. 쓰기 편하고 코드의 의도가 명확해짐
2. 다양한 상황을 고려할 수 있음 → 테스트 커버리지의 증대 
3. 전역변수를 남발하는 문제 해결  


### 작성 방법

1. Describe에 설명할 테스트 대상을 명시

```
describe('login', () => {

describe('add 함수는', () => {
```

2. Context에 테스트 대상이 놓인 상황을 명시 (with 또는 when으로 시작)

```
context('with correct accountNumber and password', () => {

context('두 개의 양수가 주어졌을 때', () => {
```

3. It에 테스트 대상의 행동을 간단하게 설명

```
it('load accountNumber information', async () => {

it('두 숫자의 합을 돌려준다', async () => {
```

<br>

## 3. React Testing Library

[React Testing Library 공식문서](https://testing-library.com/docs/react-testing-library/intro)  
UI 테스트에 특화된 라이브러리  

[jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)      
[jest-dom on GitHub](https://github.com/testing-library/jest-dom)  
추가적으로 사용할 수 있는 매처(matcher)  

React Testing Library는 거의 E2E Test처럼 사용 가능 → 브라우저에서 사용자가 실제로 사용하듯이 테스트 

> ⚠️ FE 테스트 ≠ **only** React 컴포넌트 테스트        
> 프론트엔드에서의 테스트는 종류가 다양한데, 그 중에서 React 테스트만 해서는 안 됨     
> UI는 내부의 상태를 표현해 주는 방법 중에 하나일 뿐(React가 그것을 지원하고 있지만)  
> 일일이 화면만 검사하는 것 보다 **내부 본질, 즉 상태를 검사**하는 것이 더 쉬울 수 있음         
> 유지보수를 돕기 위해 테스트 코드를 작성하는데, 테스트 코드를 잘못 작성하면(너무 많은 테스트 코드 등) 오히려 유지보수를 저해할 수 있기 때문에 주의해야 함

### E2E Test

End To End 테스트의 약자  
애플리케이션의 흐름을 처음부터 끝까지 테스트하는 것   
[(참고) 카카오엔터테인먼트 E2E 테스트 도입 경험기](https://fe-developers.kakaoent.com/2023/230209-e2e/)


### 테스트 코드 예시 

```tsx
import { render } from '@testing-library/react';

test('Greeting', () => {
  render(<Greeting name="world" />);

  screen.getByText('Hello, world!'); // 텍스트를 정확하게 모두 입력해야 함, get은 없으면 error

  screen.getByText(/Hello/); // 정규 표현식을 활용, 주로 이렇게 테스트 진행(통과 여부가 중요하기에) 

  expect(screen.queryByText(/Hi/)).not.toBeInTheDocument(); // query는 없으면 없다고 반환 
});
```
