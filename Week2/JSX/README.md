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


### JavaScript 코드와 1:1 매칭

JSX는 XML처럼 작성된 부분을 React.createElement을 쓰는 JavaScript 코드로 변환  
중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript 코드와 1:1로 매칭

> 🗼 **Babel**
>
> 변환기 [Babel](https://babeljs.io/repl) 로 확인 가능
>
> `Presets`에서 `react`를 체크하거나, `Plugins`에서 `@babel/plugin-transform-react-jsx`를 추가하면 JSX를 실험할 수 있음  
> JSX 파일에 `/* @jsx 원하는 내용 */` 주석을 추가하면 React.createElement 대신 `원하는 내용`을 쓸 수 있음    
> → 리액트가 아닌 다른 무언가를 만들 때 사용


### Babel 변환 예시 (JSX → JavaScript)

#### Ex.1

```jsx
// JSX 코드
<p>Hello, world!</p>


// 변환된 JS 코드
React.createElement("p", null, "Hello, world!");
```

* `<p>` 태그 안에 속성이 없기 때문에 null

#### Ex.2

```jsx
// JSX 코드
<Greeting name="world" />

    
// 변환된 JS 코드
React.createElement(Greeting, { name: "world" });
```

* `<Greeting>` 태그 아래의 값이 없기 때문에 세번째 인자가 없음  
* `<Greeting>`은 사용자 정의 컴포넌트이기 때문에 "Greeting"이 아닌 Greeting 으로 표기
  * 사용자가 정의했거나 import한 컴포넌트

#### Ex.3

```jsx
// JSX 코드
<div className="test">
    <p>Hello, world!</p>
    <Button type="submit">Send</Button>
</div>


// 변환된 JS 코드
React.createElement(
    "div", 
    { className: "test" }, 
    React.createElement("p", null, "Hello, world!"), 
    React.createElement(Button, { type: "submit" }, "Send")
);
```

* 최상위 태그 fragment가 없는 경우 오류 발생 
  * `add(1, 2) add(3, 4)` 와 같은 상황
  * `;` 또는 `,`로 구분해줘야 함 

#### Ex.4

```jsx
// JSX 코드
<div>
    <p>Count: {count}!</p>  
    <button type="button" onClick={() => setCount(count + 1)}>Increase</button>
</div>


// 변환된 JS 코드
React.createElement(
    "div", 
    null, 
    React.createElement("p", null, "Count: ", count, "!"), 
    React.createElement("button", { type: "button", onClick: () => setCount(count + 1) }, "Increase")
);
```

* 공백을 넣어주고 싶은 경우에는 `{' '}`를 넣기도 함 

<br>

## 2. React에서 JSX를 사용하는 목적

[Babel, JSX, 그리고 빌드 과정들](https://ko.reactjs.org/docs/faq-build.html)  
[JSX 이해하기](https://ko.reactjs.org/docs/jsx-in-depth.html)

JSX는 React.createElement를 쓰도록 JS 코드를 바꿔주는 것


<br>

## 3. Syntactic sugar

<br>

## 4. React.createElement

[createElement](https://beta.reactjs.org/reference/react/createElement)  
[JSX 없이 사용하는 React](https://ko.reactjs.org/docs/react-without-jsx.html)  

리액트 네임스페이스
* 네임스페이스 : 내부 식별자(형식, 함수, 변수 등의 이름)에 범위를 제공하는 선언적 영역

createElement()는 React 엘리먼트를 생성하여 반환  
type 인자로는 1️⃣ `태그 이름 문자열`('div', 'span' 등), 2️⃣ `React 컴포넌트 타입`, 3️⃣ `React Fragment 타입` 중 하나가 올 수 있음
  
### JSX 없이 JavaScript 사용하기 

빌드 환경에서 컴파일 설정을 하고 싶지 않을 때, `JSX 없이 React를 사용`하는 것은 특히 편리  
JSX 파일은 변환이 필요하기 때문에, 변환하고 싶지 않으면 그냥 JavaScript 파일을 사용할 수 있음  
JSX로 작성된 코드는 React.createElement() 형태로 변환 → **JSX로 할 수 있는 모든 것은 순수 JavaScript로도 가능**

#### React.createElement를 짧은 변수에 할당해 JSX 없이 React 사용하기

```js
const e = React.createElement;

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(e('div', null, 'Hello World'));
```

<br>

## 5. React Element

[엘리먼트 렌더링](https://ko.reactjs.org/docs/rendering-elements.html) 

엘리먼트는 React 앱의 가장 작은 단위  
화면에 표시할 내용을 기술

### createElement로 React Element 트리 갱신하기 

JSX 대신 React.createElement를 사용해서 **React Element 트리를 갱신**할 수 있음    
React.createElement는 DOM tree에서 node를 만드는 것과 비슷하며, 대신 엘리먼트를 생성   

> 🔎 **DOM 엘리먼트와 React 엘리먼트**  
> 
> 브라우저 DOM 엘리먼트와 달리, React 엘리먼트는 일반 객체이며(plain object) 쉽게 생성 가능  
> React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트

JSX Runtime은 _jsx란 함수를, Preact는 h란 함수를 직접 지원

```js
// Inserted by a compiler (don't import it yourself!)
import {jsx as _jsx} from 'react/jsx-runtime';

function App() {
  return _jsx('h1', { children: 'Hello world' });
}
```

<br>

## 6. React StrictMode

<br>

## 7. VDOM(Virtual DOM)이란?

<br>

### DOM이란?

### DOM과 Virtual DOM의 차이

<br>

## 8. Reconciliation(재조정) 과정은 무엇인가?
