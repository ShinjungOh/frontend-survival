# 3. React

## 1. React란?

사용자 인터페이스(웹 UI)를 만들기 위한 JavaScript 라이브러리   
**컴포넌트**로 이루어짐 (컴포넌트 : 한 가지 기능을 수행하는 UI 단위)      
[선언형 프로그래밍](https://ko.wikipedia.org/wiki/%EC%84%A0%EC%96%B8%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)  

> **선언형 프로그래밍** ✅   
> 바로 목적 서술, 목표를 명시 → 예측 가능하고 디버깅 용이         
> Ex. React  
> 
> **명령형 프로그래밍**   
> 절차를 일일이 나열, 실행할 알고리즘을 명시    
> Ex. jQuery, C, JAVA   


> * [React 공식문서](https://ko.reactjs.org/)    
>   * 옛날 정보가 아직 업데이트 되지 않은 내용도 존재     
>   * Ex. [class 컴포넌트로 쓰인 자료](https://ko.reactjs.org/docs/thinking-in-react.html#step-2-build-a-static-version-in-react)    
> * [React Beta 문서](https://beta.reactjs.org/)    
>   * ✨ 요즘 React 사용법을 다룬 문서(권장)     
>   * 베타 버전/완성도가 낮음/한국어 번역 없음  
>   * Ex. [공식문서의 내용을 함수형 컴포넌트로 설명한 자료 - thinking-in-react](https://beta.reactjs.org/learn/thinking-in-react)  

[📌 리액트의 이해를 돕는 글 ](https://overreacted.io/ko/react-as-a-ui-runtime/)

<br>

## 2. React 컴포넌트

**UI를 재사용 가능한 독립적인 여러 조각으로 나눈 것** → 유닛 테스트 하기 좋음    
`props`를 받아, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환   
컴포넌트를 정의하는 방법 : 함수 컴포넌트, 클래스 컴포넌트  

### Props

컴포넌트에 데이터를 전달하는 방법  

* 부모 컴포넌트가 자식 컴포넌트에 전달  
* 값에 이름을 붙여서 전달  
* properties의 줄임말  
* 정적, 동적 데이터(state), 컴포넌트 전달 가능  
* 자식 컴포넌트의 props가 바뀔 때마다, 부모 컴포넌트가 re-render 될 때마다 re-render됨  

<br>

## 3. React 리렌더링

### state 

[React state](https://ko.reactjs.org/docs/thinking-in-react.html#step-3-identify-the-minimal-but-complete-representation-of-ui-state)  

애플리케이션이 가지는 state

* 사용자가 입력한 검색어 
* 체크박스의 값

> 📌 **어떤 것이 state가 되어야 하는 지** 각 데이터에 세 가지 질문을 통해 결정할 수 있음  
> 
> 1. 부모로부터 props를 통해 전달되는가? 그러면 state가 아니다.  
> 2. 시간이 지나도 변하지 않는가? 그러면 state가 아니다.  
> 3. 컴포넌트 안의 다른 state나 props를 가지고 계산 가능한가? 그러면 state가 아니다.  

### 리렌더링 

state가 변경될 때 리렌더링이 발생  
리렌더링은 모든 컴포넌트의 **render를 호출**하는 것이지, React가 언마운트시키고 다시 마운트하는 것은 아님  
리렌더링은 **상태를 가지고 있는 컴포넌트**와 해당 컴포넌트의 **하위 컴포넌트**에만 영향을 미침    

[Virtual DOM](https://ko.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom)  

> 📌 **Virtual DOM Tree (가상 DOM 트리)**    
> Virtual DOM은 브라우저 API 위에 있는 JavaScript 라이브러리에서 구현되는 개념    
> 이전의 DOM Tree와 비교해서 실질적으로 어떤 부분이 업데이트 돼야 하는지 계산해서 필요한 부분만 DOM Tree에 업데이트 **( = 재조정)**  
> **렌더링 전후에 변경된 부분만을 적용**  
> ⇒ render 함수가 많이 호출되어도 성능을 크게 걱정하지 않아도 되는 이유  
> 
> * 리액트 컴포넌트가 메모리 상에 보관 
> * 60fps를 기본적으로 보장


[재조정 (Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html)

> 📌 **재조정 (Reconciliation)**  
> React는 선언적 API를 제공하기 때문에 갱신이 될 때마다 매번 무엇이 바뀌었는지를 걱정할 필요가 없음  
> React 내부에서는 **비교 (diffing) 알고리즘**을 통해 **무엇이 변경되었는지** 계산이 일어남       
> ⇒ 컴포넌트의 갱신이 **예측 가능**해지고, **빠른** 앱을 만들 수 있음

<br>

## 4. IoC(Inversion of Control)

> [제어의 역전](https://martinfowler.com/bliki/InversionOfControl.html)
>
> <em>"IoC containers의 부상으로 인해 사람들이 IoC(제어의 역전)의 의미에 혼란을 느낀다.    
> IoC(제어의 역전)의 일반적인 원칙을, IoC containers가 사용하는 IoC(제어의 역전)의 특정 스타일(예: 의존성 주입)과 혼동한다."</em>

제어의 역전  
프로그래머가 작성한 프로그램이 재사용 라이브러리의 흐름 제어를 받게 되는 디자인 패턴    

기존 → 작성한 프로그램이 외부 라이브러리의 코드를 호출해 이용  
IoC이 적용된 구조 → 외부 라이브러리의 코드가 작성한 코드를 호출   

[context API에서의 제어의 역전](https://ko.reactjs.org/docs/context.html#before-you-use-context)

### IoC의 목적

* 작업을 구현하는 방식과 작업 수행 자체를 분리
* 모듈을 제작할 때, 모듈과 외부 프로그램의 결합에 대해 고민할 필요 없이 모듈의 목적에 집중
* 다른 시스템이 어떻게 동작할지에 대해 고민할 필요 없이, 미리 정해진 협약대로만 동작하게 하면 됨
* 모듈을 바꾸어도 다른 시스템에 부작용을 일으키지 않음

### 의존성 주입

의존성 주입(dependency injection)은 하나의 객체가 다른 객체의 의존성을 제공하는 테크닉  
모듈 간 결합을 느슨하게 만듦  

<br>

## 5. Library vs Framework

### Q. React는 프레임워크인가요, 라이브러리인가요?  

공식 문서의 표현 `사용자 인터페이스를 만들기 위한 JavaScript 라이브러리`

IoC는 Framework을 정의하는 중요한 특징    
React는 IoC를 통해 상태와 업데이트가 얽힌 복잡한 상황을 간단히 선언형 UI로 구성 (React의 첫 번째 특징)  
그렇기 때문에 혼란이 발생할 수 있음    

💡 일반적으로 IoC는 IoC 컨테이너와 엮여서 DI(Dependency injection, 의존성 주입)와 동의어처럼 쓰이고,   
Next.js, Remix 등을 Framework이라고 부름  
