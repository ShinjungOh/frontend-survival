# 1. External Store

## 1. External Store

**리액트의 외부**에서 상태를 관리하는 방법  
아키텍처 관점이 아니라, 스토어가 리액트 밖(External)에 있다는 의미  
기존에는 useState 등의 hook을 이용해 처리

외부의 스토어에서 바뀐 상태를 화면에 업데이트하는 방뻡  
[forceUpdate와 같은 것이 있습니까?](https://ko.reactjs.org/docs/hooks-faq.html#is-there-something-like-forceupdate)

* forceUpdate는 Class component에서 사용하는 방법

### 예시

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commits/cf59182548faed414cff17f61d0a97b90e58bd34/2023-04-12)

#### 1. 기존의 useState

```ts
const [count, setCount] = useState(0);
```

#### 2. useReducer 이용

```ts
const [, forceUpdate] = useReducer(reducer, {x: 0});

// Reducer 함수
function reducer(state) {
    return {...state, x: state.x + 1};
}
```

#### 3. useForceUpdate 커스텀 훅 사용 ✅

```ts
export default function useForceUpdate() {
    const [, setState] = useState({});

    // UseState와 forceUpdate 함수로 useReducer와 동일 효과
    const forceUpdate = () => {
        setState({}); // 새로운 객체 생성
    };

    return forceUpdate;
}
```

### 명확한 관심사의 분리

```tsx
import useForceUpdate from '../hooks/useForceUpdate';

// Business Logic

const state = {
    count: 0,
};

function increase() {
    state.count += 1;
}

// UI

export default function Counter() {
    const forceUpdate = useForceUpdate();

    const handleClick = () => {
        increase();
        forceUpdate();
    };

    return (
        <div>
            <p>{state.count}</p>
            <button
                type='button'
                onClick={handleClick}
            >
                Increase
            </button>
        </div>
    );
}
```

React가 UI를 담당하고, 순수한 TypeScript(JavaScript)가 비즈니스 로직을 담당

#### 장점

자주 바뀌는 UI 요소에 대한 테스트 대신, 오래 유지되는(바뀌면 치명적인) **비즈니스 로직에 대한 테스트 코드**를 작성해
**유지보수에 도움**이 되는 테스트 코드를 치밀하게 작성할 수 있음


<br>

## 2. 관심사의 분리

[관심사의 분리](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%8B%AC%EC%82%AC_%EB%B6%84%EB%A6%AC)

관심사 분리(separation of concerns, SoC)는 컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙   
**관심사**란 프로그램 코드에 영향을 미치는 정보의 집합이며, 각 부문은 개개의 관심사를 해결  
**관심사의 분리**는 정보를 잘 정의된 인터페이스가 있는 코드 부분 안에 **캡슐화**(정보 숨기기의 한 수단) 시킴으로써 달성  
계층화된 디자인은 관심사 분리의 다른 구현 (Ex. 표현 계층, 비즈니스 로직 계층, 데이터 접근 계층, 퍼시스턴스 계층)  
관심사 분리는 추상화의 일종

### React Component

컴포넌트는 한 가지 기능을 수행하는 UI 단위  
리액트를 사용할 때 컴포넌트를 분리해서 사용 → 각 컴포넌트는 **관심사(기능)** 에 따라 분리된 것  
작은 컴포넌트를 모아서 더 큰 컴포넌트를 만들어냄

<br>

## 3. Layered Architecture

[Software Architecture Patterns - Layered Architecture](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html)

백엔드에서 많이 사용하는 방법  
프론트에서도 비슷하게 접근할 수 있음

### 관심사를 분리하는 기준 - 설계

Layered Architecture에서는 **사용자에게 가까운 것(UI), 사용자와 먼 것(비즈니스 로직)** 으로 구분

`🙋🏻‍♀️ 사람` → `🔘 스위치(UI)` → `⚙️ 모터(비즈니스 로직)` → `🏍 바퀴(DB)`

* 사람이 **스위치**를 조종하면(UI)
* **모터**가 움직여서(비즈니스 로직을 이용)
* **바퀴를** 굴릴 수 있음(DB에 접근하고 처리할 수 있음)

### 관심사를 분리하는 기준 - 프로세스

간단하게 `Input` → `Process` → `Output` 3단계로 코드를 적절히 구분만 해도 코드를 이해하고 유지보수하는데 크게 도움이 됨   
하나의 Output은 다시 사용자에게 Input을 요청하게 되고, 일반적인 프로그램은 다음과 같이 **계속 순환하는 구조**가 됨

1. Input: 프로그램 시작
2. Process: 프로그램 초기화
3. Output: 사용자에게 초기 UI 보여주기
4. Input: 사용자의 입력
5. Process: 사용자의 입력에 따라 처리
6. Output: 처리 결과 보여주기
7. Input: 사용자의 또 다른 입력
8. 반복

### 장점

각각은 하나의 역할(관심사)만 수행하기 때문에 복잡도가 낮아짐

### MVC와 매핑

MVC로 거칠게 매핑할 경우, 다음과 같이 볼 수 있음   
⚠️ 딱 떨어지는 것은 아니고 어느 정도의 끼워 맞추기

* Controller → Input
* Model(비즈니스 로직을 포함) → Process
* View → Output

> 🌿 **MVC 패턴**
>
> 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴  
> 구성 요소를 3가지 역할로 구분해, 개발 프로세스에서 각각의 구성 요소에만 집중할 수 있음  
> 재사용과 확장이 용이

<br>

## 4. Flux Architecture

[Flux](https://haruair.github.io/flux/docs/overview.html)

### MVC 패턴의 한계

> 🚨 View와 Model 간의 관계가 복잡해져 버그를 수정하거나 데이터 흐름을 알아보기 어려운 문제가 발생

'읽은 표시(mark seen)에 관해 많은 View가 있다면 이를 어떻게 처리해야 할까?'에 대한 해답  
(어떤 페이지에서 메시지를 읽었는데, 다른 페이지에서는 메시지가 안 읽었다고 뜨기도 하는 문제)

View에서 일어난 것이 Model에 영향을 끼치기도 하고, 그 반대도 영향을 미치는 로직이 있는 상황  
→ 데이터를 일관성 있게 View에 공유하기가 어려움(실제로 뷰가 모델을 직접 건드리거나 하는 상황은 없음)  
⇒ 해결방법으로 **데이터가 한 방향으로만 흐르도록 하는 flux 패턴이 등장**

### Flux 패턴의 탄생

Facebook(현 Meta)에서 MVC 패턴의 한계에 대안으로 내세운 아키텍처  
기존의 2-way binding(양방향 바인딩)을 썼을 때 생길 수 있는 Model-View의 복잡한 관계(전통적인 MVC에선 이런 상황을 지양)를 겨냥  
명확히 **unidirectional data flow**를 강조

> 🤝 **[양방향 바인딩](https://angular.kr/guide/two-way-binding)**
>
> 양방향 바인딩은 자식 컴포넌트에서 발생하는 이벤트를 감지하는 바인딩과, 자식 컴포넌트에 데이터를 전달하는 프로퍼티 바인딩이 결합된 형태

### Action, Dispatcher, Store

<figuare><img src="https://haruair.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png" alt="flux 패턴의 구조와 데이터 흐름"></figuare>

1. Action → 이벤트/메시지 같은 객체, ([dispatchEvent](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/dispatchEvent)
   와 비슷한 행동)
2. Dispatcher → (여러) Store로 Action을
   전달. [메시지 브로커](https://ko.wikipedia.org/wiki/%EB%A9%94%EC%8B%9C%EC%A7%80_%EB%B8%8C%EB%A1%9C%EC%BB%A4) 와 유사
3. Store (여러 개) → 받은 Action에 따라 상태를 변경, 상태 변경을 알림
4. View → Store를 구독하고 있다가 변화가 있으면 Store의 상태를 반영, 화면의 무언가를 건드리면 다시 action 발생

### 장점

단방향 데이터 흐름을 활용해 복잡한 뷰 구성을 단순화한 디자인 패턴

* 데이터 일관성이 늘어남
* 버그 찾기 쉬움
* 단위 테스팅이 쉬움

### Redux의 Flux패턴 적용사례

[Redux의 핵심](https://ko.redux.js.org/tutorials/essentials/part-1-overview-concepts/)

Redux는 **단일 스토어**를 사용함으로써 좀 더 단순한 그림을 제안

<figuare><img src="https://ko.redux.js.org/assets/images/one-way-data-flow-04fe46332c1ccb3497ecb04b94e55b97.png" alt="Redux 데이터 흐름"></figuare>

1. Action → State를 변화시킴
    * switch를 통해 action의 type을 판단, 각각의 행위를 결정
2. Store → dispatch를 통해 Action을 받고, 사용자가 정의한 reducer를 통해 State를 변경(기존 상태는 놔두고 새로운 객체 생성)
3. View → State를 반영해서 보여줌, Action을 다시 만듦

* 💡 Action을 어떻게 표현하느냐가 사용성에 큰 차이를 만듦
    * 객체/객체가 아닌 것
    * dispatch로 전달
    * Action creator 함수 생성
* 하지만 상태를 UI에 반영하는 방법은 모두 동일 ⇒ React 사용

### 3단계 프로세스와 매핑

* Input → Action + dispatch
* Process → reducer
* Output → View(React)

<br>

## 5. useReducer

[useReducer](https://ko.reactjs.org/docs/hooks-reference.html#usereducer)

useReducer가 기본형   
setState가 내부적으로 useReducer를 사용   
거의 같은 것이라고 볼 수 있음(useState의 대체 함수)  

### 사용 방법

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

<br>

## 6. useCallback

[useCallback](https://ko.reactjs.org/docs/hooks-reference.html#usecallback)

### 사용 방법

```jsx
const memoizedCallback = useCallback(() => {
    doSomething(a, b);
}, [a, b]);
```

useCallback은 콜백함수의 메모이제이션된 값을 반환  
메모이제이션된 값은 콜백의 의존성이 변경되었을 때에만 변경(useEffect의 의존성 배열과 같음)
