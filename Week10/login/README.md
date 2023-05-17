# 1. 로그인 

## 1. 용어

> 📌 9주차 용어에 추가할 내용 

- Order: 주문
    - Summary: 주문에 대한 요약 정보
    - Detail: 주문에 대한 상세 정보
    - Cart와 동일한 LineItem을 활용(오히려 Cart가 Order의 LineItem을 활용하는 것에 가까움)

<br>

## 2. 기능

> 🎯 어떤 순서로 기능을 만들 것인지는 비즈니스 우선 순위에 따라 결정

1. 상품 목록 확인 ✅
2. 상품 상세 정보 확인 ✅
3. 장바구니에 상품 담기 ✅
4. 로그인 ✅
5. 로그아웃 ✅  
6. 회원 가입 ✅
7. 주문 목록 확인
8. 주문 상세 확인
9. 주문하기 → 배송지 입력, 결제

⇒ 주문 관리를 위해 로그인을 먼저 진행하도록 순서를 변경하고, 로그아웃 기능을 추가

<br>

## 3. 화면

1. 홈 페이지 - `/`
2. 상품 목록 페이지 - `/products`
3. 상품 상세 페이지 - `/products/{id}`
4. 장바구니 페이지 - `/cart`
5. 주문 페이지 - `/order`
6. 주문 완료 페이지 - `/order/complete`
7. 주문 목록 페이지 - `/orders`
8. 주문 상세 페이지 - `/orders/{id}`
9. 로그인 페이지 - `/login`
10. 회원 가입 페이지 - `/signup`
11. 회원 가입 완료 페이지 - `/signup/complete`

<br>

## 4. JWT

[JWT](https://jwt.io/)  
[JSON 웹 토큰](https://ko.wikipedia.org/wiki/JSON_%EC%9B%B9_%ED%86%A0%ED%81%B0)  
[What is a JWT?](https://supertokens.com/blog/what-is-jwt)  
[JWT 관련 유튜브 코딩애플 영상](https://www.youtube.com/watch?v=XXseiON9CV0)

JWT(JSON Web Token)은 선택적 서명 및 선택적 암호화를 사용하여 데이터를 만들기 위한 인터넷 표준  
JWT 클레임은 **아이덴티티 제공자**와 **서비스 제공자** 간(또는 비즈니스 프로세스에 필요한 클레임)의 인가된 사용자의 아이덴티티를 전달하기 위해 사용

### 구조

헤더, 페이로드, 서명으로 이루어져 있음  
페이로드는 몇몇 클레임(claim) 표명(assert)을 처리하는 JSON을 보관

![](../../images/week10_1_jwt-structure.png)

토큰은 비공개 시크릿 키 또는 공개/비공개 키를 사용하여 서명됨

이 토큰들은 한쪽 당사자의 비공개 키(일반적으로 서버의 비공개 키)에 의해 서명이
가능하며 이로써 해당 당사자는 최종적으로 토큰이 적법한지를 확인할 수 있음

* Ex. **서버**는 `관리자로 로그인됨`이라는 클레임이 있는 토큰을 생성하여 이를 클라이언트에 제공할 수 있음   
  **클라이언트**는 해당 토큰을 사용하여 관리자로 로그인됨을 증명

```
const token = base64urlEncoding(header) + '.' + base64urlEncoding(payload) + '.' + base64urlEncoding(signature)
```

* 이 세 부분은 Base64url 인코딩을 사용하여 별도로 인코딩됨
* JWT 생성을 위해 점(.)을 사용하여 연결

### 장점

* 결과가 되는 토큰은 HTML과 HTTP로 쉽게 파싱이 가능
* 토큰은 크기가 작고 URL 안전으로 설계되어 있음
* 웹 브라우저 통합 인증(SSO) 컨텍스트에 유용

<br>

## 5. 로그인

프론트엔드 입장에서 로그인은 사용자의 username(여기서는 email)과 password를 백엔드로 전송해서
**Access Token**(여기서는 JWT를 사용)를 얻는 과정

* username/email : 이 사람은 누구이다(unique)
* password : unique함을 증명하는 암호 
* Access Token : 사용자가 증명되었으면 앞으로 계속 password 입력하지 말고 토큰을 들고오면 됨
  * 매 요청마다 토큰을 넣어서 전달  

이렇게 얻은 Access Token을 관리하는 방법은 여러 가지가 있음(쿠키, 세션 등)  
여기서는 usehooks-ts의 useLocalStorage를 사용해서 전역적으로 동기화

### `LoginPage` 생성

`LoginForm`과 `LoginFormStore`를 사용하고, Access Token이 바뀌었을 때(로그인 성공 시) 홈(/)으로 리다이렉션

```tsx
import { useEffect } from 'react';

import { useNavigate } from 'react-router-dom';

import LoginForm from '../components/login/LoginForm';

import useLoginFormStore from '../hooks/useLoginFormStore';

export default function LoginPage() {
  const navigate = useNavigate();

  const [{ accessToken }, store] = useLoginFormStore();

  useEffect(() => {
    store.reset();
  }, []);

  useEffect(() => {
    if (accessToken) {
      store.reset();
      navigate('/');
    }
  }, [accessToken]);

  return (
    <LoginForm />
  );
}
```

### `routes`에 추가

```
{ path: '/login', element: <LoginPage /> }
```

### `LoginForm` 컴포넌트 생성 


### 기본 Props 값을 지원하는 ESLint 룰 추가

```
'react/require-default-props': [2, { functions: 'defaultArguments' }],
```

<br>

## 6. 로그인 여부에 따라 다르게 보여주기

* 로그인 여부에 따라 GNB 구성이 바뀌도록 Header를 수정
* `AddToCartForm` 컴포넌트도 로그인 여부에 따라 다르게 보여주도록 구현

### GNB

Global Navigation Bar  

웹사이트 전체에 동일하게 적용되는 내비게이션 바(메인 메뉴)  
보통 웹사이트 최상단에 위치하며 어떤 페이지를 클릭해도 동일하게 보여짐  
웹사이트 전역에 해당된다는 의미로 Global 이라고 표현

### LNB

Local Navigation Bar  
 
GNB를 클릭하거나 마우스를 hover했을 때 나오는 하위 카테고리 리스트(서브 메뉴)  
특정 GNB 카테고리에 해당하는 특정 페이지로 연결되는 링크 역할   
웹사이트의 특정 지역을 한정한다는 의미로 Local 이라고 표현

<br>

## 7. API 호출할 때 AccessToken 사용

`ApiService`에 setAccessToken 메소드를 추가해서 API 호출할 때 Access Token을 전달   
💡 Axios 인스턴스를 다시 만들어 주면 다른 메소드를 수정하지 않아도 됨

```tsx
private accessToken = '';

setAccessToken(accessToken: string) {
  if (accessToken === this.accessToken) {
    return;
  }

  const authorization = accessToken ? `Bearer ${accessToken}` : undefined;

  this.instance = axios.create({
    baseURL: API_BASE_URL,
    headers: { Authorization: authorization },
  });

  this.accessToken = accessToken;
}
```

### `useAccessToken`에서 메소드 호출

```tsx
export default function useAccessToken() {
  const [accessToken, setAccessToken] = useLocalStorage('accessToken', '');

  useEffect(() => {
    apiService.setAccessToken(accessToken);
  }, [accessToken]);

  return { accessToken, setAccessToken };
}
```
