# 1. Routing

## 1. Routing(라우팅)

[라우팅](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%8C%85)

라우팅(routing)은 어떤 네트워크 안에서 통신 데이터(패킷)를 보낼 때 **최적의 경로를 선택하는 과정** → **라우터**가 이를 수행   
최적의 경로는 주어진 데이터를 가장 짧은 거리로 또는 가장 적은 시간 안에 전송할 수 있는 경로

⚡ ️가장 빠른 최적의 경로를 찾아 요청을 빠르게 전달  
데이터는 보통 출발지에서 목적지로 가는 동안 여러개의 라우터를 거치며 여러번의 라우팅을 수행(라우팅은 보통 초당 수백만번 발생)  
Ex. 🚌 대중교통 최적 방법 선택

<figure><img src="https://github.com/ShinjungOh/TIL/blob/main/Images/what-is-routing.png?raw=true" alt="routing" width="100%"></figure>

```tsx
function App() {
    const {pathname} = window.location;

    return (
        <div>
            <Header/>
            <main>
                {pathname === '/' && <HomePage/>}
                {pathname === '/about' && <AboutPage/>}
            </main>
            <Footer/>
        </div>
    );
}
```

<br>

## HTML DOM API

[HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)

HTML의 각 요소의 기능을 정의하는 인터페이스와 이들이 의존하는 지원 유형 및 인터페이스로 구성

### Location

[Window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)  
[Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

### pathname

[pathname](https://developer.mozilla.org/en-US/docs/Web/API/Location/pathname)

일반적인 웹 사이트는 URL에 따라 다른 웹 페이지를 보여줌    
하나의 웹 페이지를 하나의 컴포넌트로 만들고, URL에 따라 적절한 컴포넌트가 보이게 함으로써 구현 가능함
