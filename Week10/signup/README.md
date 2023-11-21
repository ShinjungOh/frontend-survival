# 3. 회원가입

## 1. 회원가입 로직 작성하기 

### ApiService에 signup 메소드 추가

* email : 유저를 구분하기 위함
* name : 서비스 내에서 유저가 사용하는 이름

```tsx
async signup({ email, name, password }: {
  email: string;
  name: string;
  password: string;
}): Promise<string> {
  const { data } = await this.instance.post('/users', {
    email, name, password,
  });
  const { accessToken } = data;
  return accessToken;
}
```

* 대개의 경우 API를 통해 accessToken을 받도록 되어 있음

### Store 생성

로그인과 비슷하게 `SignupFormStore`를 생성

* 서버로 보낼 때 Validation Check 값이 필요하진 않음
* 클라이언트에서만 사용자의 실수를 방지하기 위해 체크 

### SignupFormStore를 사용할 수 있는 Hook 생성

```tsx
export default function useSignupFormStore() {
  const store = container.resolve(SignupFormStore);
  return useStore(store);
}
```

<br/>

## 2. 회원가입 페이지 UI 구현하기

### SignupForm 컴포넌트 생성

```tsx
export default function SignupForm() {
  const { setAccessToken } = useAccessToken();

  const [{
    email, name, password, passwordConfirmation, valid, error, accessToken,
  }, store] = useSignupFormStore();

  useEffect(() => {
    if (accessToken) {
      setAccessToken(accessToken);
    }
  }, [accessToken]);

  const handleChangeEmail = (value: string) => {
    store.changeEmail(value);
  };
  
  // 중략 
  
  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    store.signup();
  };

  return (
    <Container>
      <h2>회원 가입</h2>
      <form onSubmit={handleSubmit}>
        <TextBox
          label="E-mail"
          placeholder="tester@example.com"
          value={email}
          onChange={handleChangeEmail}
        />
        <TextBox
          label="Name"
          value={name}
          onChange={handleChangeName}
        />
        <TextBox
          label="Password"
          type="password"
          value={password}
          onChange={handleChangePassword}
        />
        <TextBox
          label="Password Confirmation"
          type="password"
          value={passwordConfirmation}
          onChange={handleChangePasswordConfirmation}
        />
        <Button type="submit" disabled={!valid}>
          회원 가입
        </Button>
        {error && (
          <p>회원 가입 실패</p>
        )}
      </form>
    </Container>
  );
}
```

### SignupPage 구현

```tsx
export default function SignupPage() {
  const navigate = useNavigate();

  const [{ accessToken }, store] = useSignupFormStore();

  useEffect(() => {
    store.reset();
  }, []);

  useEffect(() => {
    if (accessToken) {
      store.reset();
      navigate('/signup/complete'); // 회원가입 성공 페이지 
    }
  }, [accessToken]);

  return (
    <SignupForm />
  );
}
```

### SignupCompletePage 구현 

회원가입이 완료되었을 때, HomePage로 보내지 않고 따로 회원가입 완료 페이지로 이동 

```tsx
export default function SignupCompletePage() {
  return (
    <p>
      회원 가입이 완료되었습니다.
    </p>
  );
}
```

### routes에 추가

```tsx
{ path: '/signup', element: <SignupPage /> },
{ path: '/signup/complete', element: <SignupCompletePage /> },
```

### LoginForm 컴포넌트에 회원 가입 링크 추가

```tsx
<p>
  <Link to="/signup">
    회원 가입
  </Link>
</p>
```
