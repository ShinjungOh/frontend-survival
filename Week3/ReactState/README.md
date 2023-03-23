# 2. React State

## 1. Thinking in React

> 📌 [Thinking in React](https://react.dev/learn/thinking-in-react)  
> 리액트를 이용하는 기초

### Step 3: Find the minimal but complete representation of UI state

UI 상태의 최소한의, 완벽한 표현 찾기

### Step 4: Identify where your state should live

상태가 있어야 하는 위치 확인

### Step 5: Add inverse data flow

역방향 데이터 흐름 추가

<br>

## 2. React state

[React state](https://ko.reactjs.org/docs/thinking-in-react.html#step-3-identify-the-minimal-but-complete-representation-of-ui-state)

애플리케이션이 가지는 state

* 사용자가 입력한 검색어 
* 체크박스의 값

> 📌 **React State의 조건**
> 
> 1. 부모로부터 props를 통해 전달되는가? 그러면 state가 아니다.
> 2. 시간이 지나도 변하지 않는가? 그러면 state가 아니다. → 변경되야 함 
> 3. 컴포넌트 안의 다른 state나 props를 가지고 계산 가능한가? 그러면 state가 아니다. → 계산 가능해야 함

💡 **DRY 원칙이 핵심**   
애플리케이션이 필요로 하는 **가장 최소한의 state**를 찾고 이를 통해 나머지 모든 것들이 필요에 따라 그때그때 계산되도록 만들 것  

<br>

## 3. DRY 원칙

[DRY (Don’t Repeat Yourself)](https://ko.wikipedia.org/wiki/%EC%A4%91%EB%B3%B5%EB%B0%B0%EC%A0%9C)

중복 배제(Don't repeat yourself)  
모든 형태의 정보 중복을 지양하는 원리  
똑같은 코드를 절대 Copy&Paste(반복)하지 말라는 원칙

> 📜 **소프트웨어 개발 3가지 원칙**
> 
> [DRY - Don't Repeat Yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) : 중복 배제  
> [KISS - Keep It Simple, Stupid](https://en.wikipedia.org/wiki/KISS_principle) : 관심사 분리, SRP, 불필요한 복잡성 지양    
> [YAGNI - You Ain't Gonna Need it](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) : 실제로 필요하기 전까지 구현하지 말 것  

<br>

## 4. SSOT(Single Source of Truth)

[SSOT (Single Source of Truth)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%A7%84%EC%8B%A4_%EA%B3%B5%EA%B8%89%EC%9B%90)

단일 진실 공급원

모든 데이터 요소를 한 곳에서만 제어 또는 편집하도록 조직하는 것    
모든 전역 상태는 하나의 객체(스토어) 안에 트리구조로 저장  
데이터 요소로의 가능한 연결은 모두 참조로만 이루어짐

### 장점

* 모든 상태가 하나의 스토어에 저장되어 애플리케이션이 단순해지고, 예측하기 쉬움
* Undo, Redo 등의 기능을 구현하기도 쉬워짐  

<br>

## 5. Lifting State Up

[Lifting State Up](https://ko.reactjs.org/docs/lifting-state-up.html)

다루는 상태가 너무 많으면 복잡함  
TypeScript를 잘 쓰면 직접 관리하는 상태의 수를 줄여줄 수 있음  

> 👨‍👩‍👧‍👦 **상태를 누가 관리하고, 소유해야 할까?**
> 
> (React만 사용할 경우) 해당 상태에 의존적인 컴포넌트를 **모두 포함하는 컴포넌트**가 상태를 소유해야 함

<br>

## 6. useState

[useState](https://ko.reactjs.org/docs/hooks-reference.html#usestate)

React 버전 16.8부터 React 요소로 새로 추가  
Hook을 이용하여 기존 Class 바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용 가능  

### 사용방법

```jsx
// "state"라는 새로운 상태 값을 정의
const [state, setState] = useState(initialState);
```

`상태값`과 `그 값을 갱신하는 함수`를 반환  
최초로 렌더링을 하는 동안, 반환된 state는 초기값(initialState)과 같음  

```jsx
setState(newState);
```
setState 함수는 state를 갱신할 때 사용  
새 state 값을 받아 컴포넌트 리렌더링을 큐에 등록  
useState를 통해 반환받은 첫 번째 값은 항상 갱신된 최신 state

### setState의 비동기 처리

[참고 자료](https://github.com/ShinjungOh/TIL/blob/main/React/react%20useState%20%EC%83%81%ED%83%9C%20%EA%B0%92%20%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8.md)

<img src="https://raw.githubusercontent.com/ShinjungOh/TIL/main/Images/javascript_runtime_setState.webp" alt="javascript_runtime" width="100%">

1. setState가 콜스택에 들어감 
   * 콜스택에는 한 번에 하나의 함수만 등록
2. 비동기 객체이기 때문에 바로 Web APIs로 넘어감(콜스택에서 비워짐)
   * Web APIs에서 time 처리가 끝남
3. 값이 콜백 큐로 들어감
4. 이벤트 루프를 돌면서 다시 콜스택에 들어가야 하는데, 콜스택에 이미 console.log() 존재
5. 콜 스택이 비워질 때까지 기다리다가 들어감

<br>

## 7. 1급 객체(first-class object)

[1급 객체](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)

다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체   
함수에 인자로 넘기기, 수정하기, 변수에 대입하기와 같은 연산을 지원할 때 일급 객체라고 부름  

### 일급 함수

[일급 함수](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)

함수를 **다른 변수와 동일**하게 다루는 언어는 일급 함수를 가졌다고 표현    
일급 함수를 가진 언어에서는 함수를 다른 함수에 인수로 제공하거나, 함수가 함수를 반환할 수 있으며, 변수에도 할당할 수 있음   

* 콜백 함수 : 다른 함수에 인자로 전달된 함수(제어권을 넘겨준 함수)
* 고차 함수 : 함수를 반환하는 함수

### Inverse Data Flow

하위 컴포넌트의 props로 함수를 전달(흔히 콜백 함수라고 부름)  
TypeScript(정확히는 JavaScript)는 함수가 일급(first-class) 객체  
어떤 함수를 다른 함수에 인자로 넘겨주거나, 어떤 함수를 리턴값으로 사용할 수 있음   
익명 함수, 클로저 등과 함께 사용하면 시너지가 큼

