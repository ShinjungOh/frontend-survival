# 2. Routes

## 1. 라우터

[라우터](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%84%B0)

라우터는 네트워크 사이에서 데이터를 전달하는 장치  
보통 둘 이상의 서로 다른 네트워크에 연결됨  
데이터(패킷)를 목적지로 보낼 때 최적의 경로를 결정하고 경로가 결정되면 해당 경로로 데이터를 넘겨주는 일(라우팅)을 수행  
라우터는 라우팅테이블을 기반으로 데이터를 다음 목적지에게 전달

<figure><img src="https://github.com/ShinjungOh/TIL/blob/main/Images/router_network.png?raw=true" alt="router" width="100%"></figure>

<br>

## 2. React Router

[React Router](https://reactrouter.com/)  
[Client Side Routing](https://reactrouter.com/en/main/start/overview#client-side-routing)

**Context API**를 기반으로 한 라이브러리 : 컴포넌트들에게 동일한 context(맥락)를 전달하고, 한 번에 값을 받아와서 사용

**싱글페이지 어플리케이션**을 유지하면서, url을 붙일 수 있음
  * 해당 페이지로 이동
  * 북마크 추가
  * 네비게이션 추가

### SPA (Single Page Application)

[싱글 페이지 애플리케이션](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80_%ED%8E%98%EC%9D%B4%EC%A7%80_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98)

하나의 url로 한 번 페이지가 로딩되면 그 안에서 사용자가 다른 페이지를 클릭했을 때,  
❌ 새로운 페이지가 열리는 것이 아닌(전체적인 페이지 새로고침 x),   
✅ 부분적인 내용만 업데이트(해당 컴포넌트만 변경)     
⇒ 원하는 데이터를 동적으로 받아와서 동적으로 볼 수 있도록 처리

* MPA (Multiple Page Application) : 새로고침되며 페이지를 이동시키는 방식. html 문서, 데이터 등을 받아 조립해야 함
* CSR (Client Side Rendering) : 클라이언트 측에서 페이지 렌더링
* 문제점 : 화면 북마크 불가, 뒤로가기/앞으로가기 등의 네비게이션에 포함되지 않음

### 사용 방법 

> [🔗 실습 링크 : React Router 설치 및 적용, App 테스트 코드 작성](https://github.com/ShinjungOh/2023-learn-react/commit/366960c456a0272f34fd4c92ac3136183f0f3f99)

#### 패키지 설치

```
npm i react-router-dom
```

#### React Router 사용하기 

```tsx
import {Routes, Route} from 'react-router-dom';

function App() {
    return (
        <div>
            <Header/>
            <main>
                <Routes>
                    <Route path="/" element={<HomePage/>}/>
                    <Route path="/about" element={<AboutPage/>}/>
                </Routes>
            </main>
            <Footer/>
        </div>
    );
}
```

### Route

[Routes](https://reactrouter.com/en/main/components/routes)

앱의 어디에서나 렌더링되며 `<Routes>`는 현재 위치의 **하위 경로 집합**과 일치함  
위치가 변경될 때마다 `<Routes>`는 모든 자식 경로를 살펴보고 가장 일치하는 항목을 찾아 해당 UI 분기를 렌더링  

#### 사용 방법 

`<Route>` 요소는 중첩된 URL 경로에 해당하는 중첩된 UI를 나타내기 위해 중첩될 수 있음

```tsx
<Routes>
    <Route path="/" element={<Dashboard />}>
        <Route path="messages" element={<DashboardMessages />}/>
        <Route path="tasks" element={<DashboardTasks />} />
    </Route>
    <Route path="about" element={<AboutPage />} />
</Routes>
```

[Route](https://reactrouter.com/en/main/route/route)

Route는 React Router 앱의 가장 중요한 부분   
URL 세그먼트를 구성 요소, 데이터 로드 및 데이터 변형에 연결  
경로 중첩을 통해 복잡한 애플리케이션 레이아웃과 데이터 종속성이 단순해지고 선언적이 됨

#### path

Route가 URL, link href, form action과 일치하는지 확인하기 위해 URL과 일치시킬 경로 패턴  

#### element

Route가 URL과 일치할 때 렌더링할 React 요소/구성 요소   

```tsx
<Route path="/mypage" element={<MyPage />} />
```

### Browser Router

[BrowserRouter](https://reactrouter.com/en/main/router-components/browser-router)

브라우저의 주소 표시줄에 clean URL을 사용해 현재 위치를 저장하고, 브라우저의 내장 기록 스택을 사용하여 탐색

> 🚨 **Browser Router를 사용하지 않았을 때 에러 메시지**
>
> <em>Uncaught Error: useRoutes() may be used only in the context of a `<Router>` component.</em>

#### 사용 방법

`<BrowserRouter window>`는 기본적으로 **현재 문서의 defaultView**를 사용  
다른 창의 URL에 대한 변경 사항을 추적하는 데 사용할 수도 있음 (Ex. `<iframe>`)

```tsx
import * as React from "react";
import {createRoot} from "react-dom/client";
import {BrowserRouter} from "react-router-dom";

const root = createRoot(document.getElementById("root"));

root.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>
);
```

> ✍️ **조각글** 
> 
> [🔗 [트러블 슈팅] react router v6 오류 해결](https://shinjungoh.tistory.com/entry/%ED%8A%B8%EB%9F%AC%EB%B8%94-%EC%8A%88%ED%8C%85-react-router-v6-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0)  
> [🔗 TIL : react router v6 오류 해결](https://github.com/ShinjungOh/TIL/blob/e8a438a59a5a57b270db70d1939dd6854864cf58/React/react%20router%20v6%20%EC%98%A4%EB%A5%98%20%ED%95%B4%EA%B2%B0.md)
>
> 예전에 트러블 슈팅 글을 적어둔 내용과 동일한 이슈가 강의에서 등장했다. 이제 보니 저 글 제목도 좀 더 구체적으로 적었으면 좋았으련만,
> 당시에는 정확히 어떤 카테고리에서 문제가 발생했는지 몰라서 'react router v6 오류' 라고 해두었다.
> 강의를 듣고 나서는 어떤 파트가 문제였는지 알게 되어 명쾌해진 느낌이다. 
> 더불어 테스트 코드에서는 Memory Router를 사용한다는 것까지 알게 되었다. 
> 이 내용을 몰랐다면, 테스트 코드를 작성할 때 또 똑같은 문제를 겪었을 것이고 두 번이나 트러블 슈팅을 할 뻔 했다.

### Memory Router

[MemoryRouter](https://reactrouter.com/en/main/router-components/memory-router)

⚠️ 테스트 환경에서는 Browser Router를 사용하면 실패  
💡 브라우저에는 Window가 있지만, 테스트 환경에서는 Memory Router 사용

`<MemoryRouter>`는 배열에 내부적으로 위치를 저장함    
`<BrowserHistory>, <HashHistory>`와 달리, 브라우저의 기록 스택과 같은 외부 소스에 엮이지 않음   
→ 테스트같이 기록 스택을 완벽하게 제어해야 하는 시나리오에 이상적

* initialEntries : 기본값은 ["/"] (root '/' URL의 단일 항목)
* initialIndex : 기본값은 initialEntries의 마지막 index

대부분의 React Router 테스트는 진실의 출처로 `<MemoryRouter>`를 사용하여 작성되었기 때문에  
테스트를 살펴보기만 해도 이를 사용하는 좋은 예를 볼 수 있음

#### 사용 방법

```tsx
import {render, screen} from '@testing-library/react';
import {MemoryRouter} from 'react-router-dom';

import App from './App';

const context = describe;

describe('App', () => {
    function renderApp(path: string) {
        render((
            <MemoryRouter initialEntries={[path]}>
                <App />
            </MemoryRouter>
        ));
    }

    context('when the current path is “/”', () => {
        it('renders the home page', () => {
            renderApp('/');

            screen.getByText(/Hello/);
        });
    });

    context('when the current path is “/about”', () => {
        it('renders the about page', () => {
            renderApp('/about');

            screen.getByText(/About/);
        });
    });
});
```
