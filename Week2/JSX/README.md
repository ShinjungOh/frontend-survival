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
변환된 JS 코드는 Virtual DOM 트리를 바꿔주고, 그것과 화면을 비교해서 바뀐 부분만 업데이트(재조정 과정)  
💡 이 접근방식을 통해 React의 **선언적 API**를 가능하게 만듦

<br>

## 3. Syntactic sugar

[Syntactic_sugar](https://en.wikipedia.org/wiki/Syntactic_sugar)

> <em>각 JSX 엘리먼트는 React.createElement(component, props, ...children)를 호출하기 위한 **문법 설탕**  
> 그래서 JSX로 할 수 있는 모든 것은 순수 JavaScript로도 가능</em>  

사물을 더 쉽게 표현할 수 있도록 설계된 프로그래밍 언어 내의 구문  
더 명확하고 간결하게 사용할 수 있도록 대체하여 표현하는 스타일  
더 짧고 읽고 쓰기가 쉬움  
💡 **JSX를 순수 JavaScript만으로도 표현할 수 있지만, 더 길고 복잡하기 때문에 JSX를 사용**

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

[React StrictMode](https://ko.reactjs.org/docs/strict-mode.html)

StrictMode는 자손들에 대한 부가적인 **검사와 경고**를 활성화  
개발 모드에서만 활성화되기 때문에, 프로덕션 빌드에는 영향을 끼치지 않음

* 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
* 레거시 문자열 ref 사용에 대한 경고
* 권장되지 않는 findDOMNode 사용에 대한 경고
* 예상치 못한 부작용 검사
* 레거시 context API 검사
* Ensuring reusable state

### StrictMode 활성화

```jsx
root.render((
    <React.StrictMode>
      <App />
    </React.StrictMode>
));
```

* `<App />` 컴포넌트 각각의 자손까지 검사가 이루어짐

> ⚠️ Ex. useEffect 등을 사용할 때 콘솔에 두 번씩 호출하는 경우  
> 두 번 체크해서 두 결과가 다를 경우 함수의 사이드이펙트가 크다고 경고

### React Developer Tools

[React Developer Tools](https://github.com/facebook/react/tree/main/packages/react-devtools-extensions)

1. 컴포넌트가 StrictMode 활성화 되어있지 않으면 경고   
2. 내부적으로 VDOM 트리를 비슷하게 보여줌  

<br>

## 7. VDOM(Virtual DOM)

> 🌳 트리는 프랙탈과 같음(재귀적), 트리의 구성요소는 트리  
> 매번 작은 React Element 트리, VDOM 트리가 생성됨 

VDOM은 UI의 이상적, `가상적`인 표현을 메모리에 저장하고, ReactDOM과 같은 라이브러리에 의해 `실제` DOM과 동기화하는 프로그래밍 개념  
특정 기술이라기보다는 패턴에 가까움   
React에서 `virtual DOM`이라는 용어는 보통 사용자 인터페이스를 나타내는 객체이기 때문에 React elements와 연관됨 

### VDOM을 쓰는 이유

1. 충분히 빠름 - 속도를 내세울 정도로 매우 빠른 것은 아니지만, 충분히 빠름  
2. 유지보수 용이 (핵심✨) - 예측 가능하기 때문에 유지보수가 용이 

### Virtual DOM

[Virtual DOM](https://ko.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom)

Virtual DOM은 브라우저 API 위에 있는 JavaScript 라이브러리에서 구현되는 개념      
이전의 **DOM Tree와 비교**해서 실질적으로 어떤 부분이 업데이트 돼야 하는지 계산해서 필요한 부분만 DOM Tree에 업데이트 **( = 재조정)**    
**렌더링 전후에 변경된 부분만을 적용**    
변환된 JS 코드는 Virtual DOM 트리를 바꿔주고, 그것과 화면을 비교해서 바뀐 부분만 업데이트   
⇒ render 함수가 많이 호출되어도 성능을 크게 걱정하지 않아도 되는 이유  
  
* 리액트 컴포넌트가 메모리 상에 보관  
* 60fps를 기본적으로 보장  

### DOM

[DOM(Document Object Model)](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)  
[DOM은 정확히 무엇일까?](https://wit.nts-corp.com/2019/02/14/5522)

웹 페이지에 대한 인터페이스  
뷰 포트에 무엇을 렌더링 할지 결정하기 위해 사용  
여러 프로그램들이 페이지의 콘텐츠 및 구조, 스타일을 읽고 조작할 수 있도록 API를 제공    
🌳 브라우저가 HTML 파일을 읽으면서 브라우저가 이해할 수 있는 오브젝트 트리(DOM 트리)로 변환  

#### DOM vs HTML

DOM은 원본 HTML 문서의 객체 기반 표현 방식  
둘은 서로 비슷하지만, 차이 존재

* HTML 문서 : 내용과 구조가 단순 텍스트로 구성 
* DOM : HTML 문서의 내용과 구조가 객체 모델로 변환되어, 다양한 프로그램에서 사용될 수 있음


### DOM과 Virtual DOM의 차이

fragment는 DOM에 없음 (DOM tree에 나타나지 않음)  
Vue의 template이 그나마 비슷하고, Vue에서 적극적으로 활용됨  

> 🔗 스벨트의 경우  
<em>Virtual DOM 없어도 된다, Virtual DOM을 쓴다고 빨라지는 것은 아니다.</em>

<br>

## 8. Reconciliation(재조정) 과정

[재조정 (Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html)

> 📢 새로 바뀐 데이터로 리액트 엘리먼트 만들어줘!    
> 변환된 JS 코드를 효율적으로 그리는 것은 Virtual DOM으로 가능  
> 이 접근방식이 React의 **선언적 API**를 가능하게 만듦

React는 **선언적 API를 제공**하기 때문에 갱신이 될 때마다 매번 무엇이 바뀌었는지를 걱정할 필요가 없음    
React 내부에서는 **비교 (diffing) 알고리즘**을 통해 **무엇이 변경되었는지** 계산이 일어남         
⇒ 컴포넌트의 갱신이 **예측 가능**해지고, **빠른** 앱을 만들 수 있음

### 성능 최적화

[성능 최적화](https://ko.reactjs.org/docs/optimizing-performance.html)

VDOM이 무엇이고, 왜 쓰는지 안다면 활용할 수 있는 성능 최적화 기법 

> 📃 Ex. 긴 목록을 보여줄 때  
> [1, 2, 3, 4, 5]가 [2, 3, 4, 5, 6]으로 바뀐 상황에서  
> 1이 사라지고 6이 추가된 것을 처리하기 위해 key를 잡아줌  
