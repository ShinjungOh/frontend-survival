# 1. Routing

## 1. Routing(라우팅)

[라우팅](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%8C%85)

라우팅(routing)은 어떤 네트워크 안에서 통신 데이터(패킷)를 보낼 때 **최적의 경로를 선택하는 과정** → **라우터**가 이를 수행   
**최적의 경로**는 주어진 데이터를 가장 짧은 거리로 또는 가장 적은 시간 안에 전송할 수 있는 경로

⚡ ️가장 빠른 최적의 경로를 찾아 요청을 빠르게 전달  
데이터는 보통 출발지에서 목적지로 가는 동안 여러개의 라우터를 거치며 여러번의 라우팅을 수행(라우팅은 보통 초당 수백만번 발생)  
Ex. 🚌 대중교통 최적 방법 선택

<figure><img src="https://github.com/ShinjungOh/TIL/blob/main/Images/what-is-routing.png?raw=true" alt="routing" width="100%"></figure>

### 라우팅 구현하기 

> [🔗 실습 링크 : Home, About Page 분리 및 라우팅 처리 구현](https://github.com/ShinjungOh/2023-learn-react/commit/544f208acad9e722053342c3d58f8c866464b3d7)

일반적인 웹 사이트는 URL에 따라 다른 웹 페이지를 보여줌    
하나의 웹 페이지를 하나의 컴포넌트로 만들고, URL에 따라 적절한 컴포넌트가 보이게 함으로써 구현   
(컴포넌트를 여러 조각으로 나눠서, 주소에 따라 조각을 보여주도록 하는 것)

* `location.pathname`에 따라 각기 다른 페이지를 보여주도록 처리하는 것  

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

## 2. HTML DOM API

[HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)

HTML의 각 요소의 기능을 정의하는 인터페이스와, 이들이 의존하는 지원 타입 및 인터페이스로 구성

#### HTML DOM API에 포함된 기능

* DOM을 통한 HTML 요소 접근 및 제어
* 양식(form) 데이터에 대한 접근 및 조작
* 2D 이미지의 콘텐츠 및 HTML 컨텍스트(`<canvas>`)와 상호 작용하여 그 위에 그림을 그리기
* HTML 미디어 요소(`<audio>`및 `<video>`)에 연결된 미디어 관리
* 웹 페이지에서 콘텐츠 끌어서 놓기(Drag and drop)
* 브라우저 탐색 기록에 대한 접근
* Web Components, Web Storage, Web Workers, WebSocket 및 Server-sent events와 같은 다른 API에 대한 지원 및 연결 인터페이스

### Location

[Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

**HTML DOM API** 중 페이지 기록 인터페이스  
브라우저의 기록에 대한 정보에 액세스하고 해당 기록을 통해 브라우저의 현재 탭을 앞뒤로 이동할 수 있음 

Location 인터페이스는 객체가 연결된 장소(URL)를 표현   
Location 인터페이스에 변경을 가하면 연결된 객체에도 반영되는데, Document와 Window 인터페이스가 이런 Location을 가지고 있음   
각각 `Document.location`과 `Window.location`으로 접근 가능

### Window.location

[Window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)

Window.location 프로퍼티에 접근하면 읽기 전용인 Location 오브젝트를 얻어올 수 있음    
이는 현재 도큐먼트의 로케이션에 대한 정보를 담고 있음

```
// window는 Global이기 때문에 생략 가능
// 두 가지는 동일한 것  

location
Window.location
```

> 🐦 [https://github.com/ShinjungOh](https://github.com/ShinjungOh) 페이지에서 명령어를 입력할 경우 
> 
> ▶️ location  
> Location {ancestorOrigins: DOMStringList, href: 'https://github.com/ShinjungOh', origin: 'https://github.com', protocol: 'https:', host: 'github.com', …}

### pathname

[pathname](https://developer.mozilla.org/en-US/docs/Web/API/Location/pathname)

'/' 문자 뒤 URL의 경로를 값으로 하는 DOMString  
pathname 인터페이스의 속성은 Location에 대한 URL의 경로를 포함하는 문자열  
* 경로가 없으면 pathname이 비어있음  
* 경로가 있으면 pathname 쿼리 문자열이나 조각을 포함하지 않고 URL의 경로가 뒤에 오는 초기 '/'를 포함  

> 🐦 [https://github.com/ShinjungOh](https://github.com/ShinjungOh) 페이지에서 명령어를 입력할 경우
> 
> ▶️ location.pathname  
> '/ShinjungOh'

### hash 

[hash](https://developer.mozilla.org/en-US/docs/Web/API/Location/hash)

'#' 문자 뒤 URL의 프래그먼트 식별자를 값으로 하는 DOMString

> 🐦 [https://developer.mozilla.org/ko/docs/Web/API/Location#예제](https://developer.mozilla.org/ko/docs/Web/API/Location#예제) 페이지에서 명령어를 입력할 경우
> 
> ▶️ location.hash  
> '#%EC%98%88%EC%A0%9C'

```
encodeURI('예제')

'#%EC%98%88%EC%A0%9C'
```

```
decodeURI(location.hash)

'#예제'
```
