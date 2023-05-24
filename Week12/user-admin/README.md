# 2. 로그인, 사용자 목록

## 1. 로그인

🎯 관리자 사이트는 무조건 로그인 필수   
일반 페이지와 로그인 페이지를 완전히 분리하기 

### `routes.tsx`

```tsx
const routes = [
  { path: '/login', element: <LoginPage /> },
  {
    element: <Layout />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/users', element: <UserListPage /> },
      { path: '/categories', element: <CategoryListPage /> },
      { path: '/categories/new', element: <CategoryNewPage /> },
      { path: '/categories/:id/edit', element: <CategoryEditPage /> },
      { path: '/products', element: <ProductListPage /> },
      { path: '/products/new', element: <ProductNewPage /> },
      { path: '/products/:id', element: <ProductDetailPage /> },
      { path: '/products/:id/edit', element: <ProductEditPage /> },
      { path: '/orders', element: <OrderListPage /> },
      { path: '/orders/:id', element: <OrderDetailPage /> },
      { path: '/orders/:id/edit', element: <OrderEditPage /> },
    ],
  },
];
```

### `LoginPage` 구현 

레이아웃이 적용되지 않아 스스로 꾸며줘야 한다는 점을 제외하면 기존과 동일

### `Layout` 구현 

로그인이 올바르게 된 경우에만 렌더링하도록 확실히 제약하기 

```tsx
export default function Layout() {
  const ready = useCheckAccessToken();

  if (!ready) {
    return null;
  }

  return (
    <Container>
      <Header />
      <Outlet />
    </Container>
  );
}
```

### useCheckAccessToken hook 생성 

**로그인 페이지로 리다이렉션**하거나 **ready**라고 안내하기 

```tsx
export default function useCheckAccessToken(): boolean {
  // Access Token 검증이 끝나면 ready가 된다.
  const [ready, setReady] = useState(false);

  const { accessToken, setAccessToken } = useAccessToken();

  const navigate = useNavigate();

  useEffect(() => {
    const fetchCurrentUser = async () => {
      try {
        await apiService.fetchCurrentUser();
        setReady(true);  // 👈 확인됨!
      } catch (e) {
        setAccessToken('');
      }
    };

    fetchCurrentUser();
  }, [accessToken, setAccessToken]);

  // 기존과 다른 부분
  useEffect(() => {
    if (!accessToken) {
      navigate('/login');
    }
  }, [accessToken, navigate]);

  return ready;
}
```

### 헤더 구현하기  

**관리자로 로그인**했거나 **하지 않았거나** 둘 중 하나일 수 밖에 없기 때문에 단순하게 구성할 수 있음

```tsx
export default function Header() {
  const navigate = useNavigate();

  const { setAccessToken } = useAccessToken();

  const handleClickLogout = async () => {
    await apiService.logout();
    setAccessToken('');
    navigate('/');
  };

  return (
    <Container>
      <h1>Shop Administrator</h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          {/*...*/}
          <li>
            <Button onClick={handleClickLogout}>Logout</Button>
          </li>
        </ul>
      </nav>
    </Container>
  );
}
```

<br>

## 2. 사용자 관리

> <em>사용자 관리는 사실 대단히 복잡할 수 있다.  
> 사용자와 관련된 정보가 많기 때문에 이를 모두 다루는 것 자체도 문제고,   
> 고객 지원 업무에서도 사용자는 핵심 축이 되기 때문에 제대로 된 사용자 관리는 정말 복잡한 일이다.   
> 하지만 여기서는 **단순한 CRUD**, 그것도 사용자 목록 정도만 확인하는 작업에 집중한다.   
> **SWR**를 써서 단순 작업을 얼마나 간단히 처리할 수 있는지 확인해 보자.</em>  

### 사용자 목록

🎯 간단한 CRUD로 사용자 목록을 표시하가

* 💡 내용이 많아지면 컴포넌트로 분리할 것

```tsx
export default function UserListPage() {
  const { users, loading, error } = useFetchUsers();

  // loading, error 처리

  return (
    <Container>
      <h2>Users</h2>
      <table>
        <thead>
          <tr>
            <th>이름</th>
            <th>이메일</th>
            <th>역할</th>
          </tr>
        </thead>
        <tbody>
          {users.map((user) => (
            <tr key={user.id}>
              <td>{user.name}</td>
              <td>{user.email}</td>
              <td>{user.role}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </Container>
  );
}
```

### useFetchUsers hook 생성

SWR을 바로 쓰지 않고 개별 자료형에 대응하기 위해 한번 감싸주는 형태로 구성

```tsx
export default function useFetchUsers() {
  const { data, error, loading } = useFetch<{
    users: User[];
  }>('/users');

  return {
    users: data?.users ?? [],
    error,
    loading,
  };
}
```

### useFetch hook 생성

* mutate는 바로 다음에 활용할 예정

```tsx
import useSWR from 'swr';

import { apiService } from '../services/ApiService';

const API_BASE_URL = process.env.API_BASE_URL || 'http://localhost:3000/admin';

export default function useFetch<Data>(path: string) {
  const url = `${API_BASE_URL}${path}`;

  const {
    data, error, isLoading, mutate,
  } = useSWR<Data>(url, apiService.fetcher());

  return {
    data,
    error,
    loading: isLoading,
    mutate,
  };
}
```

### ApiService에서 fetcher 제공 

[SWR에서 Axios 사용하기](https://swr.vercel.app/ko/docs/data-fetching#axios)

```tsx
fetcher() {
  return async (url: string) => {
    const { data } = await this.instance.get(url);
    return data;
  };
}
```

데이터를 변경하지만 않으면 이런 단순한 형태로 운영할 수 있음 
