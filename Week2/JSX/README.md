# 1. JSX

## 1. JSX란?

[React 공식문서 JSX 소개](https://ko.reactjs.org/docs/introducing-jsx.html)

JSX is an XML-like syntax extension to ECMAScript  
JSX는 자바스크립트를 위한 XML같은 **문법 확장**  
자바스크립트와 HTML을 합쳐서 사용할 수 있는 문법

> 🧺 **JSX는 React의 부산물**
>
> JSX는 React를 만들면서 나온 부산물 같은 것이지만, React를 사용할 때 필수는 아님  
> 또한 꼭 리액트에서만 사용해야 하는 것도 아님 <em>(Ex. Vue.js 등 다른 프레임워크에서도 사용 가능)</em>  
> 참고로, 깃허브의 텍스트 에디터 아톰의 부산물인 일렉트론으로 마이크로소프트는 VS code를 만듦


### JavaScript 코드와 1:1 매칭

JSX는 XML처럼 작성된 부분을 React.createElement을 쓰는 JavaScript 코드로 변환  
중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript 코드와 1:1로 매칭

> 🗼 **Babel**
> 
> 변환기 [Babel](https://babeljs.io/repl) 로 확인 가능
> 
> `Presets`에서 `react`를 체크하거나, `Plugins`에서 `@babel/plugin-transform-react-jsx`를 추가하면 JSX를 실험할 수 있음  
> JSX 파일에 /* @jsx 어쩌고 */ 주석을 추가하면 React.createElement 대신 “어쩌고”를 쓰게 된다.


### JSX 문법

#### 태그 닫기

여는 태그와 닫는 태그가 필요    
닫는 태그가 없을 경우, self closing tag 사용 : </>

#### 최상위 태그

[Fragments](https://ko.reactjs.org/docs/fragments.html)  
가장 바깥에 위치하는 태그  
형제 노드가 있는 경우 묶어줘야 함 : <>또는 <React.Fragment>로 묶어줌

#### 조건부 렌더링

삼항 연산자  
(조건식) ? (참 표현식) : (거짓 표현식)

#### 중괄호 {} 이용

자바스크립트 값을 사용하려면 중괄호 이용

<br>

## 2. React에서 JSX를 사용하는 목적

[Babel, JSX, 그리고 빌드 과정들](https://ko.reactjs.org/docs/faq-build.html)  
[JSX 이해하기](https://ko.reactjs.org/docs/jsx-in-depth.html)



<br>

## 3. Syntactic sugar

<br>

## 4. React.createElement

리액트 네임스페이스 

<br>

## 5. React Element

<br>

## 6. React StrictMode

<br>

## 7. VDOM(Virtual DOM)이란?

<br>

### DOM이란?

### DOM과 Virtual DOM의 차이

<br>

## 8. Reconciliation(재조정) 과정은 무엇인가?
