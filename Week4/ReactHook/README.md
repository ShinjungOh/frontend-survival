# 3. React의 Hook

## 1. React Hook

[Hook 개요](https://ko.reactjs.org/docs/hooks-overview.html)

Hook은 React 버전 16.8부터 React 요소로 새로 추가된 기능  
Hook을 이용해 기존 Class 바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용할 수 있음  
React를 쓰는 방식을 완전히 바꾼 커다란 변화 → 예전으로 돌아가는 것이 불가능

### Hook의 탄생 배경

[Hook 소개](https://ko.reactjs.org/docs/hooks-intro.html)

기존 방식에 있던 몇 가지 문제를 해결

* `Wrapper Hell (HoC)`  
컴포넌트 사이에서 상태 로직을 재사용하기 어려운 문제([render props](https://ko.reactjs.org/docs/render-props.html), [고차 컴포넌트](https://ko.reactjs.org/docs/higher-order-components.html))    
→ 💡 Hook은 계층의 변화 없이 **상태 관련 로직을 재사용**할 수 있도록 도와줌  
컴포넌트로부터 상태 관련 로직을 추상화하고, 이를 이용해 독립적인 테스트와 재사용이 가능
* `Huge Components` 이해하기 어려운 복잡한 컴포넌트  
componentDidMount, componentDidUpdate 등 상태 관련 로직, 사이드 이펙트가 있는 컴포넌트  
→ 💡 Hook을 통해 **서로 비슷한 것을 하는 작은 함수의 묶음**으로 컴포넌트를 나누는 방법 사용
* `Confusing Classes` 사람과 기계를 혼동시키는 Class  
React에서 Class를 사용하려면 JavaScript의 this가 어떻게 작동하는지 알아야 함    
JavaScript의 this는 대부분의 다른 언어에서와는 다르게 작동하기 때문에 혼란 유발, 코드의 재사용성과 구성을 매우 어렵게 만듦     
→ 💡 Hook은 Class없이 React 기능들을 사용하는 방법을 제시

### Class Component vs Function Component

|        차이점        |                                                                               Class                                                                               |                            Function                            |
|:-----------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------:|
|       생성 방법       |                                                              리액트에서 제공하는 컴포넌트라는 클래스를 extends, 상속해서 만듦                                                              |                          함수로 간단하게 생성                           |
|        용도         |                                                       컴포넌트에 state가 있고, 상태에 따라 주기적으로 업데이트 되야 하면 클래스 컴포넌트 사용                                                        |         컴포넌트에 state가 없고, 항상 정적으로 데이터가 표기될 시 함수 컴포넌트 사용         |
| lifecycle methods | [lifecycle methods](https://react.dev/reference/react/Component#adding-lifecycle-methods-to-a-class-component) 존재<br/>컴포넌트의 상태에 따라서 함수를 구현해 놓으면 리액트가 알아서 불러주는 메소드 |             라이프사이클 메소드 X<br/>함수는 한 가지의 일을 수행하는 단위              |
|    React Hooks    |                                        [점진적 적용 전략](https://ko.reactjs.org/docs/hooks-intro.html#gradual-adoption-strategy)                                        | 함수 컴포넌트 안에서 state를 가질 수 있고, 라이프사이클 메소드도 사용 가능(React 16.8 Ver.) |


#### 기존
- 상태를 가진 컴포넌트는 Class Component로 만들고, props만 사용하는 재사용이 용이한 작은 컴포넌트는 Function Component로 작성
- Redux에서도 비슷한 구분이 존재
    - [Presentational and Container Components - Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

#### 현재
- 그냥 Function Component만 사용
- 상태 관리 유무를 바로 알기 어려움 = 신경쓰지 않아도 됨
- 복잡한 요소는 전부 Hook으로 격리 및 재사용 가능

### 고차 컴포넌트, HoC (Higher-Order Components)

[고차 컴포넌트(HoC)](https://ko.reactjs.org/docs/higher-order-components.html)

고차 컴포넌트는 컴포넌트를 가져와 **새 컴포넌트를 반환**하는 함수  
컴포넌트는 props를 UI로 변환하는 반면에, 고차 컴포넌트는 컴포넌트를 새로운 컴포넌트로 변환

고차 컴포넌트는 컴포넌트 로직을 **재사용**하기 위한 React의 고급 기술  
고차 컴포넌트(HOC)는 React API의 일부가 아니며, React의 구성적 특성에서 나오는 패턴

컴포넌트를 다른 컴포넌트로 감싸고 props로 내려주는 기술   
불필요한 컴포넌트가 계속 생기는 단점

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

<br>

## 2. Hooks

[Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html)

기본 3가지 

* useState(State Hook) ⇒ React의 State
* useEffect ⇒ Side-effect
* useContext

### useState

[useState](https://ko.reactjs.org/docs/hooks-reference.html#usestate)  
[State Hook 사용하기](https://ko.reactjs.org/docs/hooks-state.html)

useState(State Hook) ⇒  React의 State를 다룸

```jsx
// "state"라는 새로운 상태 값을 정의
const [state, setState] = useState(initialState);
```

상태값과 그 값을 갱신하는 함수를 반환  
최초로 렌더링을 하는 동안, 반환된 state는 초기값(initialState)과 같음

```jsx
setState(newState);
```

setState 함수는 state를 갱신할 때 사용  
새 state 값을 받아 컴포넌트 리렌더링을 큐에 등록  
useState를 통해 반환받은 첫 번째 값은 항상 갱신된 최신 state  

```jsx
setState((prevState) => prevState + 1);
```

기존 상태를 얻고싶을 때는 함수를 넣어줌

### useEffect

[useEffect](https://ko.reactjs.org/docs/hooks-reference.html#useeffect)  
[Effect Hook 사용하기](https://ko.reactjs.org/docs/hooks-effect.html)  
[useEffect 완벽 가이드 - by Dan Abramov](https://overreacted.io/ko/a-complete-guide-to-useeffect/)

useEffect ⇒ Side-effect를 다룸

```jsx
useEffect(didUpdate);
```

일부 component는 리액트가 아닌 외부 시스템과 동기화해야 함 → 사이드 이펙트라고 함   
그런 경우가 아니라면 useEffect를 쓰지 않는 것이 좋음 

렌더링 이후 해야 할 일, 즉 React의 외부와 관련된 일을 처리  
기본적으로 **렌더링 때마다 실행**되므로, **의존성 배열**을 통해 언제 이펙트를 실행할지 지정 가능(불필요한 경우에 건너뛰기)

함수를 리턴함으로써 종료 처리를 할 수 있음

> 🤔 **side effects** 
> 
> 데이터 가져오기, 구독(subscription) 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것 등
> 
> 1. 정리(clean-up)를 이용하는 Effects → 함수를 반환  
> 외부 데이터에 구독(subscription)을 설정해야 하는 경우에 메모리 누수가 발생하지 않도록 정리
> 
> 
> 2. 정리(Clean-up)를 이용하지 않는 Effects → 어떤 것도 반환하지 않음  
> React가 DOM을 업데이트한 뒤 추가로 코드를 실행해야 하는 경우  
> Ex. 네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리(clean-up)가 필요 없는 경우


> 🤓 **팁**
> 
> React의 class 생명주기 메소드에 친숙하다면, 
> useEffect Hook을 componentDidMount와 componentDidUpdate, componentWillUnmount가 합쳐진 것으로 생각해도 좋음


### useContext

[useContext](https://ko.reactjs.org/docs/hooks-reference.html#usecontext)

context 객체를 받아 그 context의 현재 값을 반환, context 값을 편리하게 조회  
💡 context의 값에 접근할 때, Consumer를 사용하지 않아도 됨  
useContext로 전달한 인자는 context 객체 그 자체이어야 함

```jsx
const value = useContext(MyContext);

// 틀린 사용: useContext(MyContext.Consumer)
// 틀린 사용: useContext(MyContext.Provider)
```

> **클래스에서의 static contextType = MyContext 또는 <MyContext.Consumer>와 동일**   
> 클래스 메소드에서도 context에 넣어 둔 함수 호출 가능  
> 한 클래스에서 하나의 context만 사용 가능

#### useContext를 호출한 컴포넌트는 context 값이 변경되면 항상 리렌더링 발생
* 메모이제이션을 사용하여 최적화 가능

#### context의 현재 값은 Hook을 호출하는 컴포넌트에 가장 가까운 <MyContext.Provider>의 value prop에 의해 결정
* <MyContext.Provider>가 갱신되면 Hook은 provider에게 전달된 가장 최신의 context value를 사용하여 렌더
* 상위 컴포넌트에서 React.memo 또는 shouldComponentUpdate를 사용하더라도 useContext를 사용하고 있는 컴포넌트 자체에서부터 다시 렌더링

> ⚠️ **프롭스 드릴링을 피하기 위해 사용하는 것, 전역 상태관리가 아님**  
> 
> React에서 데이터를 전달하는 기본 원칙인 단방향성 때문에 Props Drilling(프롭스 드릴링) 문제가 발생  
> * 여러 컴포넌트에 동일한 값을 접근할 수 있도록 만들어주는 api(통로의 개념)
> * 자주 변경되는 데이터를 관리하려면 용도에 맞게 구분해서 사용하면 됨

### useRef

[useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)

```jsx
const refContainer = useRef(initialValue);
```

useRef는 .current 프로퍼티로 전달된 인자(initialValue)로 초기화된 변경 가능한 ref 객체를 반환  
반환된 객체는 컴포넌트의 전 생애주기를 통해 유지될 것

> **ref는 전달되지 않음** 
> 
> 고차 컴포넌트는 모든 props를 래핑된 컴포넌트에 전달하는 것이 원칙이지만, refs에서는 작동하지 않음   
> 이는 React에서 ref가 실제 prop이 아닌 key처럼 특별하게 취급되기 때문  
> 컴포넌트가 HOC의 결과인 엘리먼트에 ref를 추가하는 경우,   
> ref는 래핑된 컴포넌트가 아닌 가장 바깥쪽 컨테이너 컴포넌트의 인스턴스를 나타냄  

### useLayoutEffect

[useLayoutEffect](https://ko.reactjs.org/docs/hooks-reference.html#uselayouteffect)

- useLayoutEffect → useEffect와 조금 다름 
- 어느 타이밍에 정확하게 실행되는지를 볼 때 차이가 존재 

useEffect와 동일하지만, 모든 DOM 변경 후에 동기적으로 발생   
DOM에서 레이아웃을 읽고 동기적으로 리렌더링하는 경우에 사용할 것  
useLayoutEffect의 내부에 예정된 갱신은 브라우저가 화면을 그리기 이전 시점에 동기적으로 수행 

먼저 `useEffect`를 사용해 보고 문제가 있다면 `useLayoutEffect`를 사용해 보는 것이 권장됨 

<br>

## 3. useEffect 타이머 예제

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/144950dcc123fb9c573fc728b481d8f1f100b902)

React의 외부에 우아하게 접근  
이 정도는 useEffect를 안 쓴다고 크게 문제가 되지 않지만, 이렇게 쓰는 습관을 들이면 좋음  
되도록이면 함수를 이펙트 안으로 옮기기 

```jsx
useEffect(() => {
    document.title = `Now: ${new Date().getTime()}`;
});
```

타이머를 on/off하는 기능을 그냥 만들면 문제가 발생

```jsx
// TimerControl.tsx

function Timer() {
  useEffect(() => {
    setInterval(() => {
        document.title = `Now: ${new Date().getTime()}`;
    }, 100);
  });

  return (
      <p>Playing</p>
  );
}

export default function TimerControl() {
  const [playing, setPlaying] = useState(false);

  const handleClick = () => {
    setPlaying(!playing);
  };

  return (
      <div>
        {playing ? (
            <Timer />
        ) : (
            <p>Stop</p>
        )}
        <button type="button" onClick={handleClick}>Toggle</button>
      </div>
  );
}
```

* 토글을 눌러 setInterval을 끄려고 해도 계속 실행되는 오류  

### 종료 처리

함수를 리턴해서 clean-up

```jsx
// TimerControl.tsx

useEffect(() => {
    const savedTitle = document.title;
    
    const id = setInterval(() => {
        document.title = `Now: ${new Date().getTime()}`;
        }, 100);
    
    return () => {
        document.title = savedTitle;
        clearInterval(id);
    };
});
```

### 의존성 배열

의존성 배열에서 아무 것도 지정하지 않으면 **맨 처음에 딱 한번**만 실행  
주로 API를 호출해서 데이터를 얻을 때 사용  
빈 배열([])을 전달한다면 effect 안에 있는 props와 state는 항상 초기값을 가지게 될 것

```jsx
export default function TimerControl() {
    const [count, setCount] = useState(0);
    
    useEffect(() => {
        console.log('effect');
        }, []);

  return (
        <div>
          <button type='button' onClick={() => setCount(count + 1)}>Increase</button>
        </div>
    )
}
```

🚨 `onClick={() => setCount(count + 1)}`을 `onClick={setCount(count + 1)}`로 잘못 쓴다면 무한 리렌더링 발생  

### 처음에 한번만 실행하기

의존성 배열이 없을 경우 무한 리렌더링  

products를 얻는 곳에서 사용(데이터를 불러오는 맨 처음) 

```jsx
// App.tsx

const [products, setProducts] = useState<Product[]>([]);

useEffect(() => {
    const fetchProducts = async () => {
        const url = 'http://localhost:3000/products';
        const response = await fetch(url);
        const data = await response.json();
        setProducts(data.products);
    };
    fetchProducts();
}, []);
```

#### 의존성 배열을 이용해 Fetch할 때 주의사항

[Fetching data](https://react.dev/learn/synchronizing-with-effects#fetching-data)

```jsx
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  return () => {
    ignore = true;
  };
}, [userId]);
```

<br>

## 4. React StrictMode

[React StrictMode](https://ko.reactjs.org/docs/strict-mode.html)

StrictMode는 자손들에 대한 부가적인 검사와 경고를 활성화  
**개발 모드**에서만 활성화되기 때문에, 프로덕션 빌드에는 영향을 끼치지 않음

* 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
* 레거시 문자열 ref 사용에 대한 경고
* 권장되지 않는 findDOMNode 사용에 대한 경고
* 예상치 못한 부작용 검사
* 레거시 context API 검사
* Ensuring reusable state

### 사용 방법

```jsx
root.render((
    <React.StrictMode>
        <App/>
    </React.StrictMode>
));
```

* `<App />` 컴포넌트 각각의 자손까지 검사가 이루어짐

### 예상치 못한 부작용 검사

> ⚠️ **useEffect 등을 사용할 때 두 번 실행되는 문제(콘솔에 두 번씩 호출 출력)**
>
> [예상치 못한 부작용 검사](https://ko.reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)  
> <React.StrictMode>로 컴포넌트 전체를 감쌀 경우, 예상치 못한 사이드 이펙트를 찾으려고 Effect 등을 두 번씩 실행       
> 두 번 체크해서 두 결과가 다를 경우 함수의 사이드 이펙트가 크다고 경고   
> 평소에는 큰 문제가 없지만, API 등을 사용하면 이상하다고 느낄 수 있으니 참고  
