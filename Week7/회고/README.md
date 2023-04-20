# 한 주를 마치며

## 회고

이번 주차는 라우팅에 대해 배웠다. 분량은 다른 주에 비해 적었지만, 기존에 라우팅 처리를 이론보다는 실전으로 배웠기 때문에 이론 학습이 필요했음을 
공부를 하고 나서야 깨달았다. 피드백 받은대로 인사이트를 넣어 노트정리를 했더니 조금 더 **나의 것**이 된 느낌이다.

리액트 라우터를 공식문서 위주로 학습했다. 처음 배울 때 일단 사용법을 먼저 익히느라 이론에 약했다. 
이론부터 공부하고 실전에 돌입하는 것과는 다른 맛인데, 아는 내용이라 그런지 공식 문서의 글자가 눈에 더 잘 들어왔다.
**사용법을 알고 있으면 어려워보이는 내용이 수월하게 읽힌다**는 장점이 있다.

**리액트 라우터가 Context API를 활용한 라이브러리**라는 것을 제대로 느꼈다.
Context API에서 Provider는 기본값을 사용하지 않기 때문에 value가 없으면 오류가 발생하는데, RouterProvider도 그럴까?
실습하던 IDE를 껐는데, 회고를 마저 작성하고 실험해 봐야겠다. 

매 파트마다 테스트 코드를 작성하는 방법과 주의점이 같이 나와서 좋았다. 오류를 해결하기 위해서 맨 땅에 헤딩했을 시간이 세이브됐다. 
테스트 코드에 스며들고 있다. **일단 인풋을 많이 넣으면, 어떻게든 된다**는 걸 체험하고 나니까 '이건 어려워서 지금은 안 볼래' 따위는 변명이 되었다.
테스트도 일단 많이 접하고 나면 나의 테스트 포비아도 완화되길 바란다. 왠지 심리학에서의 노출 치료법이 이런 맥락인가 싶다.

이번주는 분량이 적어 쉬어가는 느낌으로 공부했는데, 시간이 평소보다 널널해서 이전에 미처 끝내지 못한 과제를 보충해야겠다. 리프레쉬되고 산뜻한 한 주다. 

<br>

## 💡 새로 알게된 내용

- Routing
  - HTML DOM API
  - encodeURI/decodeURI
- Routes
  - Memory Router
- Router
  - RouterProvider
  - createBrowserRouter
  - createMemoryRouter
- Navigation 
  - History.pushState
  - NavLink


## 🔎 이해가 깊어진 내용

- Routing
  - 라우팅(routing)
- Routes
  - SPA (Single Page Application)
  - Browser Router
- Router
  - React Router
- Navigation
  - Hook으로서의 useNavigate


## 🤔 학습이 더 필요한 내용

- Routes 
  - Client Side Routing, Client Side Rendering
- Router
  - RouterProvider를 이용한 라우팅 구현하기 
- Navigation
  - 상황마다 적절한 태그로 라우팅 구현하기 
