# 3. 상품 상세 보기

## 1. 상품 상세

상품 상세 정보를 얻어서 보여주는 페이지를 준비한다. 
여기서는 단순하게 처리하기 위해, 상품을 찾을 수 없는 경우를 따로 구분해서 표현하지 않고 그냥 일반 에러로 표시한다(당연히 구분해주는 게 더 좋다).

```tsx
export default function ProductDetailPage() {
  const params = useParams();

  const { loading, error } = useFetchProduct({
    productId: String(params.id),
  });

  if (loading) {
    return (
      <p>Loading...</p>
    );
  }

  if (error) {
    return (
      <p>Error!</p>
    );
  }

  return (
    <ProductDetail />
  );
}
```

장바구니 담기 기능 때문에 prop drilling 문제가 발생할 수 있어서, Page에서 product를 내려주지 않게 했다.
ProductDetail 컴포넌트에서 product만 얻어서 활용하자.

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

Fetch 작업으로 준비가 된 Store를 활용하기 위해 useProductDetailStore 훅을 만든다.

```tsx
export default function useProductDetailStore() {
  const store = container.resolve(ProductDetailStore);
  return useStore(store);
}
```

ProductDetailStore를 만든다.

```tsx
@singleton()
@Store()
export default class ProductDetailStore {
  product: ProductDetail = nullProductDetail;

  loading = true;

  error = false;

  async fetchProduct({ productId }: {
    productId: string;
  }) {
    this.startLoading();

    try {
      const product = await apiService.fetchProduct({ productId });
      this.setProduct(product);
    } catch {
      this.setError();
    }
  }

  @Action()
  private startLoading() {
    this.product = nullProductDetail;
    this.loading = true;
    this.error = false;
  }

  @Action()
  private setProduct(product: ProductDetail) {
    this.product = product;
    this.loading = false;
    this.error = false;
  }

  @Action()
  private setError() {
    this.product = nullProductDetail;
    this.loading = false;
    this.error = true;
  }
}
```

적당한 Null Object도 만든다([참고](https://refactoring.com/catalog/introduceSpecialCase.html)).

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

이제 useFetchProduct 훅을 만들 수 있다.

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

Images 컴포넌트를 만든다.

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

Description를 만든다. 임의로 key를 잡아주기 위해 꼼수를 사용했다.

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
