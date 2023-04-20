# 4. Navigation

## 1. Web APIs 

### History

[History](https://developer.mozilla.org/ko/docs/Web/API/History)

History 인터페이스는 **브라우저의 세션 기록**, 즉 현재 페이지를 불러온 탭 또는 프레임의 방문 기록을 **조작**할 수 있는 방법을 제공

### History.pushState

[History.pushState()](https://developer.mozilla.org/ko/docs/Web/API/History/pushState)

HTML5부터 새로 등장한 메소드  
브라우저의 세션 기록 스택에 상태를 추가  
링크를 가로챔

* 주어진 데이터를 지정한 제목(제공한 경우 URL도)으로 세션 기록 스택에 추가 
* 데이터는 DOM이 불투명(opaque)하게 취급하므로, 직렬화 가능한 모든 JavaScript 객체를 사용할 수 있음   
* Safari를 제외한 모든 브라우저는 title 매개변수를 무시

🔎 이전에는 새로 로딩되는 것을 막기 위해 주소에 `#`, `#!` 등을 붙여서 사용했음

#### 사용 방법

```js
history.pushState(state, title[, url]);
```

* state
  * 아무 객체나 넣을 수 있음  
  * 새로운 세션 기록 항목에 연결할 상태 객체

* url (Optional)
  * 새로운 세션 기록 항목의 URL
  * 지정하지 않은 경우 문서의 현재 URL을 사용

```tsx
const handleClick = event => {
  event.preventDefault();
  const state = {};
  const title = '';
  const url = '/about';

  history.pushState(state, title, url);
}
```

* `<a>` 태그에 onClick으로 넘겨주기  

> ✍️ **조각글**
>
> 평소 `<Link>` 태그를 그냥 사용하곤 했었는데, History.pushState 라는 방법으로 직접 처리해줄 수 있다는 것을 새로 알게 되었다.
> React가 Class 컴포넌트에서 함수 컴포넌트로 패러다임이 전환된 것 처럼, 자바스크립트 비동기 처리의 혁신 `async/await`의 등장처럼
> 기술은 항상 편리하고 쉬운 방향으로 진화한다는걸 느낀다. 그리고 이전의 불편함을 모르고 있는 내가 다행이라는 생각도 들고. 
> 한편으론 지금의 편리함에도 수명이 있다고 생각하면 앞으로 따라잡아야 할 신기술에 아득해지기도 한다. 


<br>

## 2. React Router

> [🔗 실습 링크 : Navigate 적용, 테스트 코드 작성 및 whatwg-fetch 설치](https://github.com/ShinjungOh/2023-learn-react/commit/c5982555dffd2792bf86ed5ee8a0d684b7c98364)  
> [🔗 실습 링크 : useNavigate 이용해 리다이렉션 처리하기](https://github.com/ShinjungOh/2023-learn-react/commit/1bc5461e5805584bc6c7547fd5a9538429decded)

### Link

[Link](https://reactrouter.com/en/main/components/link)

사용자가 클릭하거나 탭하여 다른 페이지로 이동할 수 있게 해주는 요소   
`<Link>`는 연결된 리소스를 가리키는 href가 있는 접근 가능한 `<a>` 요소를 렌더링

History.pushState를 직접 하지 않아도 React Router에서 다 해주는 기능  

* 페이지 전환 시 주소 변경
* 내용 변경

#### 사용 방법

```tsx
import {Link} from 'react-router-dom';

export default function Header() {
    
    return (
        <header>
          <nav>
            <ul>
              <li><Link to="/">Home</Link></li>
              <li><Link to="/about">About</Link></li>
            </ul>
          </nav>
        </header>
    );
}
```

### NavLink

[NavLink](https://reactrouter.com/en/main/components/nav-link)

네비게이션용 링크  
**활성**, **보류** 여부를 알 수 있는 `<Link>` 태그  
탐색 메뉴, 현재 선택된 항목을 표시할 때 유용  

#### `<NavLink>` 처리된 버튼 클릭 시 요소 변화 

기본적으로 active 클래스는 `<NavLink>`가 활성 상태일 때 추가됨  
CSS로 스타일 지정 가능 

```html
<a class="active" href="/" aria-current="page">Home</a>
```

#### 사용 방법

```tsx
function Header() {
    return (
        <header>
          <nav>
            <ul>
              <li><NavLink to="/">Home</NavLink></li>
              <li><NavLink to="/about">About</NavLink></li>
            </ul>
          </nav>
        </header>
    );
}
```

### Navigate

[Navigate](https://reactrouter.com/en/main/components/navigate)

무조건 리다이렉션 발생     
`<Navigate>`는 렌더링될 때 현재 위치를 변경    
어떤 작업을 수행하고 리다이렉션을 발생시킬 때 (Ex. Logout 처리)

#### 사용 방법

```tsx
import { Navigate } from 'react-router-dom';

export default function LogoutPage() {
    return (
        <Navigate to="/" />
    );
}
```

#### 테스트

```tsx
    context('When the current path is "/logout")', () => {
        it('redirects to the home page', () => {
            renderRouter('/logout');
            
            screen.getByText(/Welcome/);
        });
    });
```

💡 테스트에서 `ReferenceError: Request is not defined` 에러가 나면 **whatwg-fetch**를 임포트해서 해결

> 🚨 **에러 메시지**
>   
> ▶️ `RUNS`  src/routes.test.tsx  
> return new Request(url, init);  
> ^
> 
> ReferenceError: Request is not defined

#### 해결 방법

```
npm i -D whatwg-fetch
```

`setupTests.ts`에 `import 'whatwg-fetch'`

### useNavigate

[useNavigate](https://reactrouter.com/en/main/hooks/use-navigate)

Hook을 이용해 redirect을 편리하게 처리   

#### replace 

뒤로가기 방지  
로그인을 해야 접근할 수 있는 페이지의 경우, 로그아웃을 해서 유저 토큰 값이 없을 때 사용

```tsx
 navigate('/',  { replace: true });
```

navigate의 url로 넘어간 후에는 뒤로가기를 하더라도 이전 페이지로 돌아올 수 없음

#### 사용 방법

```tsx
import { useNavigate } from 'react-router-dom';

export default function Header() {
    const navigate = useNavigate();
    
    const handleClickLogout = () => {
        navigate('/');
    };
    
    return (
        <header>
          <ul>
            <li><Link to='/'>Home</Link></li>
            <li><Link to='/about'>About</Link></li>
            <li>
              <button type='button' onClick={handleClickLogout}>
                Log out
              </button>
            </li>
          </ul>
        </header>
    );
}
```
