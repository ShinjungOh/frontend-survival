# 3. Redux 따라하기

## 1. Redux

[Redux 시작하기](https://ko.redux.js.org/introduction/getting-started/)

> <em>자바스크립트 앱을 위한 **예측 가능한** 상태 컨테이너(상태 관리)</em>

리액트 상태 관리 라이브러리    

Flux 패턴, CQRS, Event Sourcing의 개념을 사용해서 만든 라이브러리  
**Flux 패턴의 단방향성**을 차용했기에 Redux 내에서 발생하는 상태 변화는 모두 **예측 가능**

컴포넌트의 상태 업데이트 관련 로직을 다른 파일로 분리시켜 효율적으로 관리  
컴포넌트끼리 똑같은 상태를 공유해야 할 때, 여러 컴포넌트를 거치지 않고 상태 값을 전달 또는 업데이트

1. 전역 상태 관리
2. Props drilling 문제 해결

### 장점

* 코드의 유지 보수성 증가
* 작업 효율 극대화
* 개발자 도구 지원
* 미들웨어 기능을 제공, 비동기 작업을 효율적으로 관리

<br>

## 2. Redux 3가지 원칙

### 단일 스토어

> 하나의 애플리케이션 안에는 하나의 스토어만 가진다.

리덕스 내의 모든 전역 상태는 하나의 객체(스토어) 안에 트리구조로 저장  
모든 상태가 하나의 스토어에 저장되어 **애플리케이션이 단순해지고, 예측하기 쉬움**      
Undo, Redo 등의 기능을 구현하기도 쉬워짐

⚠️ 여러 개의 스토어를 사용할 수도 있지만, 상태 관리가 복잡해질 수 있어서 지양

### 읽기 전용 상태

> 리덕스 상태는 읽기 전용이므로, 불변성을 유지해야 한다.

상태 업데이트 시 기존 객체의 **불변성을 유지**해야 함  
리액트와 마찬가지로 불변성을 유지하기 위해 스프레드 연산자나 immer(불변성 관리 라이브러리) 사용

* 불변성을 유지해야 하는 이유
    * 얕은 비교 검사를 하기 때문 - 좋은 성능을 유지할 수 있음
    * **안정성과 예측 가능성을 증대시킴**

* 리덕스 상태를 변화시키는 유일한 방법
    * 💡 순수함수인 리듀서 함수를 통해서 수행
    * Action 객체를 Dispatch를 통해서 전달하는 것 - 여러 곳에서 동시에 데이터를 수정하면서 발생하는 race condition 문제 등이 일어나지 않음
    * 🚨 **스토어에 직접 접근**해서 상태를 수정하는 등의 행위는 허용 불가

* Action을 통해서 변화의 의도를 표현
    * Action은 단순한 형태의 객체
    * 추적, 로깅, 저장 등의 동작이 용이해서 디버깅이 쉬움
    * 테스트 코드 작성 용이
    
### 순수 함수

> 변화를 일으키는 리듀서 함수는 순수 함수여야 한다.

* 💡 순수 함수
    * **동일한 Input**을 받았을 때, 항상 **동일한 Output**을 반환하는 함수
    * 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받아서, 새로운 state를 리턴하는 순수 함수
    * 파라미터 외의 값에는 의존하면 안 됨
    * 이전 상태를 수정하는 것이 아니라, 기존의 상태를 이용해서 새로운 상태를 만들어냄
    
* 리듀서 함수 사용 시, 함수 내에 **사이드이펙트**가 없어야 함
    * 리듀서 함수 내에서 랜덤 값 생성, Date 함수로 현재 시간 가져오기, 네트워크 요청 등을 하면 
  **파라미터가 같아도(같은 input) 다른 결과(다른 output)** 를 낼 수 있기 때문에 사용 불가
    * 리듀서 함수 바깥에서 처리해야 함 - 액션 생성 과정, 리덕스 미들웨어 등

* 스토어를 관리하는 리듀서도 한 개여야 함
    1. 각기 다른 관심사가 하나의 함수에 모두 들어가면 유지보수에 좋지 않음
    2. 애플리케이션이 커지면 여러개의 리듀서 함수(slice reducer)로 분리해서 코드를 작성하고
    3. 하나의 리듀서(root reducer)로 통합하는 방식을 활용
    
<br>

## 3. Redux 구성 요소 

### 액션

상태에 어떠한 변화가 필요할 때 액션 발생  
하나의 객체로 표현됨  

### 액션 생성 함수

액션 객체를 만들어 주는 함수  
어떤 변화를 일으킬 때마다 액션 객체를 만들어야 함 → 함수로 만들어서 관리

### 리듀서

변화를 일으키는 함수  
액션을 만들어서 발생시키면 리듀서가 **현재 상태**와 **전달받은 액션 객체**를 파라미터로 받아 옴  
두 값을 참고해서 **새로운 상태**를 만들어 반환  

### 스토어

프로젝트에 리덕스를 적용하기 위해 스토어를 생성  
한 개의 프로젝트는 **하나의 스토어**만 가짐  
스토어 안에는 현재 애플리케이션 상태, 리듀서, 내장 함수 등이 존재 

###  디스패치

스토어의 내장 함수 중 하나  
**액션을 발생시키는 함수**  
dispatch(action) 형태로 액션 객체를 파라미터로 넣어서 호출  

디스패치가 호출되면 스토어는 리듀서 함수를 실행시켜서 새로운 상태를 만듦

### 구독

스토어의 내장 함수 중 하나  
subscribe 함수 안에 리스너 함수를 파라미터로 넣어서 호출하면,  
리스너 함수가 액션이 디스패치되어 상태가 업데이트 될 때마다 호출됨  

<br>

## 4. Redux 따라하기 

> [🔗 실습 링크 : BaseStore, Store 생성 및 useDispatch 분리](https://github.com/ShinjungOh/2023-learn-react/commit/0fa7f63f6768e0a865451f727ca45000a79e1183)  
> [🔗 실습 링크 : useSelector 함수 분리](https://github.com/ShinjungOh/2023-learn-react/commit/25df636ce1810383ee60a7671ec70cb3d42f25a2)


### useDispatch

[useDispatch](https://ko.redux.js.org/tutorials/fundamentals/part-5-ui-react/#dispatching-actions-with-usedispatch)

컴포넌트 내부에서 스토어의 내장 함수인 dispatch를 사용할 수 있도록 함  
컨테이너 컴포넌트에서 액션을 넘겨줄 떄(디스패치 할 때) 사용

```js
const dispatch = useDispatch(); 
dispatch({ type: 'SAMPLE_ACTION' });

// 예시 
const dispatch = useDispatch();
const onIncrease = useCallback(() => dispatch(increase()), [dispatch]);
```

💡 성능을 최적화 할 때, useCallback으로 액션을 디스패치하는 함수를 감싸기

### useSelector

[useSelector](https://ko.redux.js.org/tutorials/fundamentals/part-5-ui-react/#reading-state-from-the-store-with-useselector)

connect 함수를 사용하지 않고 리덕스 상태를 조회할 수 있음  
값을 얻어다 쓸 때 사용 

```js
const 결과 = useSelector(상태 선택 함수);
// 상태 선택 함수는 mapStateToProps와 동일 형태

// 예시 
const count = useSelector(state => state.count);
const number = useSelector(state => state.counter.number);
```


<br>

## 5. Reflect

[Reflect](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect)  

중간에서 가로챌 수 있는 JavaScript 작업에 대한 메서드를 제공하는 내장 객체  
Reflect는 함수 객체가 아니므로 생성자로 사용할 수 없음 

#### 사용 예시 

* 객체가 특정 속성을 가지고 있는지 검사하기
* 객체 자체 키를 반환하기
* 객체에 새로운 속성 추가하기

### Reflect.get()

[Reflect.get()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect/get)

객체의 속성을 가져오는 메소드

```js
const f = Reflect.get(reducers, action.type);
```

