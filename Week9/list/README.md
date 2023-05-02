# 2. 목록 보기

## 1. 상품 목록

> [🔗 실습 링크 : useFetchProducts hook 구현](https://github.com/ShinjungOh/2023-learn-react/commit/cb5eebec28342d8d38bb61d0a020e7b11fc7638d)  
> [🔗 실습 링크 : Products UI 구현](https://github.com/ShinjungOh/2023-learn-react/commit/b2b5b9a697d61eb1c2e3e486036fb47763244470)  
> [🔗 실습 링크 : ]()

**상품 목록을 얻어서** 표시하는 **화면을 만들기**

1. 상품 목록 얻기 - API 서버에서 
2. 상품 목록 보여주기 - React로 

전자는 useFetchProducts 훅으로, 
후자는 Products 컴포넌트로 구현하고, ProductListPage에서 이 둘을 조합

* 단일 책임 원칙 
* 테스트 용이

### ProductListPage 구현 

```tsx
// ProductListPage.tsx

import Products from '../components/product-list/Products';

import useFetchProducts from '../hooks/useFetchProducts';

export default function ProductListPage() {
    // Todo 1. 상품 목록 얻기
    const { products } = useFetchProducts();

    // Todo 2. 화면에 보여주기
    return (
        <div>
            <h2>Products</h2>
            <Products products={products} />
        </div>
    );
}
```

### useFetchProducts hook 생성 

```tsx
// useFetchProducts.ts

const apiBaseUrl = 'https://shop-demo-api-01.fly.dev';

export default function useFetchProducts() {
    type Data = { 
        products: ProductSummary[]; 
    };

    const { data } = useFetch<Data>(`${apiBaseUrl}/products`);

    return {
        products: data?.products ?? [], 
        loading: !data, 
        error,
  };
}
```

* BaseUrl은 추후 환경 변수로 분리 : 재활용과 유지보수가 쉬워짐 
* 필요한 경우 loading과 error를 같이 내보낼 수 있음 

### Products 컴포넌트 구현 

Product 컴포넌트 분리 

```tsx
type ProductsProps = {
  products: ProductSummary[];
}

export default function Products({ products }: ProductsProps) {
  if (!products.length) {
    return null;
  }

  return (
    <Container>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            <Link to={`/products/${product.id}`}>
              <Product product={product} />
            </Link>
          </li>
        ))}
      </ul>
    </Container>
  );
}
```

### numberFormat 유틸리티 함수 구현  

> 🌎 [Intl](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Intl)  
> 각 언어에 맞는 문자비교, 숫자, 시간, 날짜비교를 제공하는, ECMAScript 국제화 API를 위한 네임 스페이스  
> 원하는대로 커스텀이 가능 

숫자를 읽기 좋게 보여주도록 numberFormat 유틸리티 함수를 준비  
간단한 유틸 함수이기 때문에 테스트도 같이 작성 

```tsx
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}
```

### 상품 목록을 Store로 관리

> [🔗 참고 자료](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week6/tsyringe)

```tsx
// stores/ProductsStore.ts

const apiBaseUrl = '...';
    
@singleton()
@Store()
export default class ProductsStore {
  products: ProductSummary[] = [];

  async fetchProducts() {
    this.setProducts([]);

    const { data } = await axios.get(`${apiBaseUrl}/products`);
    const { products } = data;

    this.setProducts(products);
  }

  @Action()
  setProducts(products: ProductSummary[]) {
    this.products = products;
  }
}
```

* reflect-metadata import
* tsconfig.json 파일에 decorator 설정 주석 해제

### useFetchProducts hook 변경

```tsx
export default function useFetchProducts(): {
  products: ProductSummary[];
} {
  const store = container.resolve(ProductsStore);

  const [{ products }] = useStore(store);

  useEffectOnce(() => {
    store.fetchProducts();
  });	

  return { products };
}
```

<br>

## 2. 카테고리 목록

헤더에 카테고리 목록을 보여주자.

```tsx
export default function Header() {
  const { categories } = useFetchCategories();

  return (
    <Container>
      <h1>Shop</h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
            {!!categories.length && (
              <ul>
                {categories.map((category) => (
                  <li key={category.id}>
                    <Link to={`/products?categoryId=${category.id}`}>
                      {category.name}
                    </Link>
                  </li>
                ))}
              </ul>
            )}
          </li>
          <li>
            <Link to="/cart">Cart</Link>
          </li>
        </ul>
      </nav>
    </Container>
  );
}
```

바로 Store를 준비한다.

```tsx
@singleton()
@Store()
export default class CategoriesStore {
  categories: Category[] = [];

  async fetchCategories() {
    this.setCategories([]);

    const categories = await apiService.fetchCategories();

    this.setCategories(categories);
  }

  @Action()
  setCategories(categories: Category[]) {
    this.categories = categories;
  }
}
```

API 호출을 모아주는 ApiService를 만든다. API의 base URL을 지정하기 위해 환경변수를 활용한다.

```tsx
const API_BASE_URL = process.env.API_BASE_URL
                     || 'https://shop-demo-api-01.fly.dev';

export default class ApiService {
  private instance = axios.create({
    baseURL: API_BASE_URL,
  });

  async fetchCategories(): Promise<Category[]> {
    const { data } = await this.instance.get('/categories');
    const { categories } = data;
    return categories;
  }

  async fetchProducts(): Promise<ProductSummary[]> {
    const { data } = await this.instance.get('/products');
    const { products } = data;
    return products;
  }
}

export const apiService = new ApiService();
```

마지막으로, useFetchCategories 훅을 만든다.

```tsx
export default function useFetchCategories() {
  const store = container.resolve(CategoriesStore);

  const [{ categories }] = useStore(store);

  useEffectOnce(() => {
    store.fetchCategories();
  });

  return { categories };
}
```

## 카테고리별 상품 목록

ProductListPage 컴포넌트에서 categoryId를 얻는다.

```tsx
export default function ProductListPage() {
  const [params] = useSearchParams();

  const categoryId = params.get('categoryId') ?? undefined;

  const { products } = useFetchProducts({ categoryId });

  return (
    <div>
      <h2>Products</h2>
      <Products products={products} />
    </div>
  );
}
```

카테고리 ID를 쓰도록 훅을 변경.

```tsx
export default function useFetchProducts({ categoryId }: {
  categoryId: string;
}): {
  products: ProductSummary[];
} {
  const store = container.resolve(ProductsStore);

  const [{ products }] = useStore(store);

  useEffect(() => {
    store.fetchProducts({ categoryId });
  }, [store, categoryId]);

  return { products };
}
```

Store도 변경.

```tsx
async fetchProducts({ categoryId }: {
  categoryId?: string;
}) {
  this.setProducts([]);

  const products = await apiService.fetchProducts({ categoryId });

  this.setProducts(products);
}
```

API Service도 변경.

```tsx
async fetchProducts({ categoryId }: {
  categoryId?: string;
} = {}): Promise<ProductSummary[]> {
  const { data } = await this.instance.get('/products', {
    params: { categoryId },
  });
  const { products } = data;
  return products;
}
```

사실 강의를 준비할 때는 처음부터 이렇게 만들기는 했다. 처음부터 고민해서 바로 만들어도 되고, 일단 만들고 이렇게 고쳐나가도 된다. 테스트 코드가 있으면 이런 변경 작업을 할 때 더 자신감을 얻을 수 있다.
