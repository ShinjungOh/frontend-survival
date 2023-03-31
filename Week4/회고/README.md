# 한 주를 마치며

## 회고

미지의 영역이었던 백엔드 관련 내용이 등장했다. **Express로 웹 서버를 구축**하는 과정을 실습을 통해 진행했다. 
간단해 보였지만 혼자서 해보려고 하면 오래 걸릴 것 같다. 백엔드에서 받아온 데이터를 어떻게 불러오는지도 학습했다. 
모르고 있었다면 실전에서 당황했을 CORS 관련 내용과 CORS 미들웨어를 사용하는 에러 해결법도 새로 배웠다.

React Hook의 탄생 배경을 공식문서를 통해 학습했는데 이 과정에서 **Class 컴포넌트와 Function 컴포넌트의 차이점**을 알게 되었다.
나는 처음 리액트를 접했을 때부터 Function 컴포넌트를 사용했기 때문에 이전의 환경이 어땠는지 잘 알지 못한다. 
이번 기회에 hook이 기존의 Class 컴포넌트가 가진 단점을 어떻게 보완하는지, 
이러한 패러타임의 변화가 리액트로 개발하는 것을 얼마나 편리하게 만들었는지 엿볼 수 있었다. 
인간은 항상 쉽고 간편한 방법을 찾기 위해 끊임없이 노력한다는 생각이 들었다. 

다른 hook에 비해 비교적 데면데면했던 useRef를 깊이 공부했다. 개발할 때 직접 사용해보면서 감을 익혀야겠다. 
개념어들이 어려워서 쉽게 손이 가지 않던 클로저도 이번에 들여다 보았다.
**'어떤 함수의 지역 변수를 참조하는 내부 함수가 외부로 전달될 때, 어떤 함수의 LexicalEnvironment가 가비지 컬렉팅 되지 않는 현상'** 을 클로저라고 한다.
이해가 되기 전후에 읽으면 각각 와닿는 느낌이 확 다른 문장이다.

**Custom Hook**을 사용하면 어떤 점이 좋은지, 어떻게 하면 만들 수 있는지 등을 배웠다. 거창한 이름에 비해 굉장히 간단했고, 
함수를 분리해 사용하는 것은 이미 해오고 있던 방식이었다. 
이번 강의로 **usehooks-ts**의 존재를 처음 알게 되었는데 편리한 hook이 많았다. 사용 방법도 간단하고 신세계가 열린 듯 했다. 
이번 주차에 배운 다양한 hook을 직접 사용해봐야겠다.

<br>

## 💡 새로 알게된 내용

- Express
  - 서버를 구축하는 방법 
  - nodemon 
- Fetch API & CORS
  - ReadableStream
  - CORS
- React의 Hook
  - Hook의 탄생 배경
  - useLayoutEffect
- useRef & Custom Hook
  - ref와 useRef
  - Custom Hook의 특징 
- usehooks-ts
  - usehooks-ts의 존재와 사용 방법


## 🔎 이해가 깊어진 내용

- Express
  - HTTP Method(CRUD)
- React의 Hook
  - Class Component vs Function Component
  - useEffect
  - React StrictMode
- useRef & Custom Hook
  - useRef의 특징
  - 클로저(closure)
  - Custom Hook을 나누고 사용하는 방법


## 🤔 학습이 더 필요한 내용

- Express
  - 필딩 제약 조건
- Fetch API & CORS
  - Fetch API
- React의 Hook
  - 고차 컴포넌트(HoC)
- useRef & Custom Hook
  - useRef의 사용 방법
- usehooks-ts
  - 다양한 hook의 종류와 사용 방법
  - SWR
  - React Query

