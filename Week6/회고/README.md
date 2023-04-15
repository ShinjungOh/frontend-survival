# 한 주를 마치며

## 회고

데브로드 과정 중 가장 어렵다는 **External Store**에 대해 배웠다. 
리팩토링이 물 흐르듯 지나가서 강의를 보며 실습도 진행하려니 따라가기가 벅찼다.
어느정도의 작은 프로젝트에서는 그럭저럭 괜찮지만, 조금만 규모가 커져도 상태관리 때문에 머리가 복잡해지기 일쑤였다.
이번 주차의 학습 내용을 잘 익혀서 상태를 잘 관리할 수 있도록 해야겠다. 

TSyringe를 처음 알게 되었다. 이름을 센스있게 잘 지었다고 생각했다.
**Props Drilling 문제를 해결**할 수 있는 방법으로 Context API 밖에 모르고 있었는데
TSyringe도 이 문제를 해결할 수 있다고 한다. 이 과정에서 TypeScript Decorators라는 기능도 알게 됐다.

Redux는 예전에 공부만 하고서 직접 프로젝트에 사용해본 적은 없었는데, 이번에 Redux 따라하기 강의를 통해 좀 가까워 진 것 같다.
특히 **useDispatch, useSelector**를 구현하는 과정에서 리덕스가 어떤 식으로 구성되어 있고, 어떻게 작동하는지 알 수 있었다.
**action, dispatch, reducer, state의 관계**를 잘 이해하고 있어야겠다.
상태 변화가 필요할 때 action에 type을 담아 액션을 발생시킨다. dispatch(action) 형태로 액션을 넣어 호출하면 
reducer 함수는 현재 상태와 전달받은 액션 객체를 받아 새로운 상태를 만들어 반환한다. 이것이 store의 상태를 업데이트하면,
React에서는 UI가 변화하게 된다. 

usestore-ts를 사용하는 방법도 배웠다. 내부적으로 어떻게 동작하는지 아직 이해하지 못했다. 
내가 만약 어떤 프로젝트에 usestore-ts를 사용해서 구현한다면, 
**왜 이런 라이브러리를 사용했으며 이 라이브러리의 특징과 장단점은 무엇인지 설명할 수 있어야** 한다.
어떤 기술을 아무 고민과 이해 없이 사용하는 행위는 지양해야 한다고 배웠기 때문에 내부적으로 어떻게 구성되어 있는지 
더 학습하는 시간을 가져야겠다.

<br>

## 💡 새로 알게된 내용

- External Store
  - Layered Architecture
  - useReducer
- TSyringe
  - TSyringe 특징과 사용 방법
  - reflect-metadata
  - TypeScript Decorators
- Redux 따라하기
  - Reflect
- usesotre-ts
  - Immer
  - usesotre-ts 사용 방법


## 🔎 이해가 깊어진 내용

- External Store
  - 관심사의 분리
  - Flux Architecture
  - useCallback
  - Memoization
- TSyringe
  - Props Drilling
  - Singleton (싱글톤)
- Redux 따라하기
  - Redux 3가지 원칙


## 🤔 학습이 더 필요한 내용

- External Store
  - Action, Dispatcher, Store의 관계 
- TSyringe
  - 의존성 주입
- Redux 따라하기
  - Redux 따라 구현하는 실습 과정 
- usesotre-ts
  - useSyncExternalStore
