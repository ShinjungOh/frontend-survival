# 3. 상품 상세 보기

## 1. 상품 상세 데이터 받아오기 

🎯 상품 상세 정보를 얻어서 보여주는 페이지  

* 상품을 찾을 수 없는 경우를 따로 구분해서 표현하면 좋음
* 지금은 구분하지 않고 그냥 일반 에러로 표시할 것 

### `ProductDetailPage.tsx` 작성

> [🔗 실습 링크 : 상품 상세 보기 로딩 및 에러 처리 구현](https://github.com/ShinjungOh/2023-learn-react/commit/12305de9e05cb8d56af2767b6b51463bfb5a8d53)

useFetchProduct에서 loading과 error를 받아 화면에 표현하기

* Loading 
* Error
* ProductDetail

### useProductDetailStore hook 생성 

* src/stores
* tsyringe의 container를 이용해 `ProductDetailStore`를 활용

```tsx
export default function useProductDetailStore() {
  const store = container.resolve(ProductDetailStore);
  return useStore(store);
}
```

### `ProductDetailStore.ts` 생성 

#### 구현 내용 

1. 로딩 시작
   * loading = true
   * error = false

2. apiService.fetchProduct({productId})
   * ApiService에서 fetchProduct 구현
   * data를 그대로 product로 내보내기

3. 로딩 끝 
   * loading = false, error는 상태 봐서 처리 
   * product


#### 타입 지정

```
// 초기값을 null로 잡을 수 있음 
product: ProductDetail | null = null;

// null을 계속 체크해야하거나 에러를 피하기 위해 ✅
product: ProductDetail = nullProductDetail;
```

* Null Object를 생성 

#### @Action() 추가 

* startLoading
* setProduct
* setError

### Null Object 생성 

[Special Case(null object pattern)](https://refactoring.com/catalog/introduceSpecialCase.html)

types.ts의 하단에 생성하거나 그 외의 곳도 가능 

```tsx
export const nullProductDetail: ProductDetail = {
  id: '',
  category: { id: '', name: '' },
  images: [],
  name: '',
  price: 0,
  options: [],
  description: '',
};
```

### useFetchProduct hook 생성

* src/hooks

```tsx
export default function useFetchProduct({ productId }: {
  productId: string;
}): {
  loading: boolean;
  error: boolean;
} {
  const [{ loading, error }, productDetailStore] = useProductDetailStore();

  useEffect(() => {
    productDetailStore.fetchProduct({ productId });
  }, [productDetailStore, productId]);

  return { loading, error };
}
```

<br>

## 2. 상품 상세 페이지 UI 구현하기 

> [🔗 실습 링크 : 상품 상세 보기 페이지 UI 구현](https://github.com/ShinjungOh/2023-learn-react/commit/2b5eda3ee9d253556d3224b5c8cf2fd941d28fba)

* 장바구니 담기 기능 때문에 prop drilling 문제가 발생할 수 있어서, Page에서 product를 내려주지 않게 처리
* ProductDetail 컴포넌트에서 product만 얻어서 활용할 것 

### `ProductDetail.tsx` 컴포넌트 구현  

* src/components/product-detail/ProductDetail.tsx

#### 구현 내용  

* Store 에서 상품 정보 얻기 - fetch는 page에서 처리
* 보여주기

```tsx
const Container = styled.div`
  display: flex;
  justify-content: space-between;

  aside {
    width: 38%;
  }

  article {
    width: 60%;
  }
`;

export default function ProductDetailView() {
  const [{ product }] = useProductDetailStore();

  return (
    <Container>
      <aside>
        <Images images={product.images} />
      </aside>
      <article>
        <h2>{product.name}</h2>
        <AddToCartForm />
        <Description value={product.description} />
      </article>
    </Container>
  );
}
```

### Images 컴포넌트 생성 

```tsx
const Thumbnail = styled.img.attrs({
  alt: 'Product Image',
})`
  display: block;
  width: 100%;
  aspect-ratio: 1/1;
`;

type ImagesProps = {
  images: Image[];
}

export default function Images({ images }: ImagesProps) {
  const [image] = images;

  if (!image) {
    return null;
  }

  return (
    <Thumbnail src={image.url} />
  );
}
```

### Description 컴포넌트 생성

* 임의로 key를 잡아주기 위해 컴포넌트 밖에서 `function key(text, index)`를 생성

```tsx
function key(value: string, index: number) {
  return `${index}-${value}`;
}

const Container = styled.div`
  li {
    min-height: 1rem;
    line-height: 1.4;
  }
`;

type DescriptionProps = {
  value: string;
}

export default function Description({ value }: DescriptionProps) {
  if (!value.trim()) {
    return null;
  }

  const lines = value.split('\n');

  return (
    <Container>
      <ul>
        {lines.map((line, index) => (
          <li key={key(line, index)}>
            {line}
          </li>
        ))}
      </ul>
    </Container>
  );
}
```
