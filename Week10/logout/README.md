# 2. 로그아웃

## 1. Access Token 확인

사이트에 접속하거나 새로 고침 등을 했을 때 Access Token을 확인하고, 사용자 정보를 얻는 작업이 필요  
회원 정보를 얻는 API가 실패하면 Access Token에 문제가 있는 상황이니 로그아웃 시키도록 구현

### `ApiService`에 fetchCurrentUser 메소드 추가

```tsx
async fetchCurrentUser(): Promise<{
  id: string;
  name: string;
}> {
  const { data } = await this.instance.get('/users/me');
  const { id, name } = data;
  return { id, name };
}
```

### `useCheckAccessToken` hook 생성

```tsx
export default function useCheckAccessToken(): void {
  const { accessToken, setAccessToken } = useAccessToken();

  useEffect(() => {
    const fetchCurrentUser = async () => {
      try {
        await apiService.fetchCurrentUser();
      } catch (e) {
        setAccessToken('');
      }
    };

    fetchCurrentUser();
  }, [accessToken, setAccessToken]);
}
```

### 가장 밖에 있는 `Layout`에서 useCheckAccessToken을 호출

💡 사이트 접속할 때마다 항상 볼 수 있는 곳

```tsx
export default function Layout() {
  useCheckAccessToken();

  return (
    <Container>
      <Header />
      <Outlet />
    </Container>
  );
}
```

<br>

## 2. 보안 이슈 

[프런트엔드 보안 입문서](https://frontendmastery.com/posts/frontend-security-primer/)

🚨 프론트엔드에서의 보안은 XSS 공격을 막느냐에, 못 막느냐가 관건  
프론트엔드 보안 공격의 60%는 일종의 XSS 공격인 것으로 추정  

### XSS

사이트 간 스크립팅, 크로스 사이트 스크립팅(Cross-site scripting XSS)  
주로 다른 웹사이트와 정보를 교환하는 식으로 작동하므로 **사이트 간 스크립팅**이라고 부름  

웹 애플리케이션에서 많이 나타나는 취약점의 하나   
웹사이트 관리자가 아닌 이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다.
 
주로 여러 사용자가 보는 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어짐  
이 취약점은 웹 애플리케이션이 사용자로부터 입력 받은 값을 제대로 검사하지 않고 사용할 경우 나타남   
이 취약점으로 해커가 사용자의 정보(쿠키, 세션 등)를 탈취하거나, 자동으로 비정상적인 기능을 수행하게 할 수 있음 

### 대응 방안

[XSS 공격과 쿠키의 HttpOnly](https://velog.io/@alang/XSS-%EA%B3%B5%EA%B2%A9%EA%B3%BC-%EC%BF%A0%ED%82%A4%EC%9D%98-HttpOnly)

쿠키는 자바스크립트를 통해 조회가 가능  
클라이언트에 저장된 쿠키 데이터도 XSS 공격을 통해 탈취가 가능 

> 🍪 **HttpOnly Cookie**
> 
> 클라이언트에서 HTTP 통신 외에는 쿠키에 접근 불가하도록 구현  
> HttpOnly Cookie는 클라이언트에서 자바스크립트를 이용한 접근이 금지됨 
> 서버에서는 HTTP 응답 헤더로 Set-cookie를 통해 클라이언트 측에 쿠키를 생성하도록 요청할 수 있음  
> 이때 이 쿠키에 옵션을 부여할 수 있는데, 그 중 하나가 HttpOnly


<br>

## 3. 로그아웃

기존에 사용하던 토큰을 복사해놨어도, 로그아웃하면 무효화    
새로 로그인해서 토큰을 재발급받아야 함  

### `ApiService`에 로그아웃 API를 호출하는 메소드 추가

```tsx
async logout(): Promise<void> {
  await this.instance.delete('/session');
}
```

### 로그아웃 버튼이 있는 `Header` 컴포넌트 수정

```tsx
export default function Header() {
  const navigate = useNavigate();

  const { accessToken, setAccessToken } = useAccessToken();

  const { categories } = useFetchCategories();

  const handleClickLogout = async () => {
    await apiService.logout();
    setAccessToken('');
    navigate('/');
  };

  // …(중략)…
}
```
