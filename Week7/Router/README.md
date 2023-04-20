# 3. React Router

## 1. RouterProvider

[RouterProvider](https://reactrouter.com/en/main/routers/router-provider)

React Router 버전 6.4부터 지원하는, **라우터 객체**를 만들어서 쓰는 방법  
모든 데이터 라우터 개체는 이 구성 요소로 전달되어 앱을 렌더링하고 나머지 데이터 API를 활성화

> ✍️ **조각글**
>
> React Router가 `Context API를 기반으로 한 라이브러리`라는 것이 물씬 느껴지는 파트였다.
> Context API를 사용해본 적이 많지 않아서 그런지, 
> '굳이 RouterProvider를 사용해 라우팅을 구현해야 할까?'라는 의문이 든다.
> 2강에서 배웠던 기존의 방식으로 구현하면 어느 순간 불편해지는 일이 생기기 때문일까? 
> 페이지가 많이 나오지 않는 작은 프로젝트같은 경우에는 그냥 이전 방식을 사용했었는데, 
> 연습 차원에서 RouterProvider도 사용해 봐야겠다. 

<br>

## 2. createBrowserRouter

[createBrowserRouter](https://reactrouter.com/en/main/routers/create-browser-router)

React Router 웹 프로젝트에 권장되는 라우터  
DOM History API를 사용하여 URL을 업데이트하고 기록 스택을 관리  
loaders, actions, fetchers 등과 같은 v6.4 데이터 API를 활성화

### routes

children 프로퍼티에 중첩된 경로가 있는 Route 객체의 배열

```tsx
createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    loader: rootLoader,
    children: [
      {
        path: "events/:id",
        element: <Event />,
        loader: eventLoader,
      },
    ],
  },
]);
```

<br>

## 3. 사용 방법 

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/bd5e6f51b0536680eca50facc1a75a3614f3c2f5)

> 📋 `App.tsx`가 하는 일
>
> 1. 전체적인 레이아웃 구성
> 2. 라우팅 구성

### 라우팅 정보 별도의 파일로 분리 

#### 컴포넌트 레이아웃 잡기

```tsx
// src/components/Layout.tsx

import Header from './Header';
import Footer from './Footer';

import { Outlet } from 'react-router-dom';
 
export default function Layout() {
    return (
        <div>
            <Header/>
            <Outlet/>
            <Footer/>
        </div>
    );
}
```

#### 레이아웃 적용

💡 테스트에서도 routes 정보를 필요로 하기 때문에, 테스트를 편리하게 하기 위해 분리하는 것이 좋음 

```tsx
// src/routes.tsx

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

import Layout from './components/Layout';

const routes = [
    {
        element: <Layout/>,
        children: [
            {path: '/', element: <HomePage/>},
            {path: '/about', element: <AboutPage/>},
        ],
    },
];

export default routes;
```

### `App.tsx`에서 불러오기 

> 🚨 **RouterProvider와 BrowserRouter를 동시에 쓸 경우 에러 메시지**
> 
> <em>caught Error: You cannot render a `<Router>` inside another `<Router>`.
> You should never have more than one in your app.</em>

```tsx
// src/App.tsx

import {createBrowserRouter, RouterProvider} from 'react-router-dom';
import routes from './routes';

const router = createBrowserRouter(routes);

export default function App() {
    return (
        <RouterProvider router={router}/>
    );
}
```

💡 App 컴포넌트를 거치지 않고 바로 브라우저 라우터를 만들어서 사용해도 됨

```tsx
// src/main.tsx

import {createBrowserRouter, RouterProvider} from 'react-router-dom';

import routes from './routes';

const router = createBrowserRouter(routes);

root.render((
    <React.StrictMode>
        <RouterProvider router={router}/>
    </React.StrictMode>
));
```

<br>

## 4. createMemoryRouter

[createMemoryRouter](https://reactrouter.com/en/main/routers/create-memory-router)

App.tsx를 테스트하는 것은 매우 어렵기 때문에, `routes.test.tsx`로 변경  
routes가 라우팅 정보를 가지고 있음   

createMemoryRouter는 브라우저의 기록을 사용하는 대신 메모리 라우터는 메모리에서 자체 기록 스택을 관리   
Storybook과 같은 테스트 및 컴포넌트 개발 도구에 주로 유용하지만,
브라우저가 아닌 모든 환경에서 React Router를 실행하는 데에도 사용할 수 있음  

### 테스트 코드 작성  

메모리 라우터 만들어서 테스트

```tsx
describe('routes', () => {
    function renderRouter(path: string) {
        const router = createMemoryRouter(routes, {initialEntries: [path]});
        render(<RouterProvider router={router}/>);
    }
    
    context('when the current path is “/”', () => {
        it('renders the home page', () => {
            renderRouter('/');
            
            screen.getByText(/Hello/);
        });
    });
    
    context('when the current path is “/about”', () => {
        it('renders the about page', () => {
            renderRouter('/about');
            
            screen.getByText(/About/);
        });
    });
});
```
