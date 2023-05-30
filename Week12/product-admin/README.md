# 5. 상품 관리

## 1. 상품 목록

🎯 상품 목록 상세보기, 추가하기, 수정하기, 삭제하기 구현

* 기본적인 fetch 작업에선 SWR을 사용 
* 폼을 관리하는 부분은 React Hook Form 대신 usestore-ts를 사용해서 기존 코드와 비교해서 어색한 부분이 있을 수 있음

### `ProductListPage` 생성 

기존의 페이지와 유사  
주문 목록 페이지보다 단순 

### `useFetchProducts` hook 생성 

```tsx
export default function useFetchProducts() {
  const {
    data, error, loading, mutate,
  } = useFetch<{
    products: ProductSummary[];
  }>('/products');

  return {
    products: data?.products ?? [],
    error,
    loading,
    async refresh() {
      mutate();
    },
  };
}
```

* 기존에는 refresh 안에서 apiService를 통해 create 했다면, 여기서는 store를 만들어 처리 

<br>

## 2. 상품 상세

### `ProductDetailPage` 생성

```tsx
export default function ProductDetailPage() {
  const params = useParams();

  const { product, loading, error } = useFetchProduct({
    productId: String(params.id),
  });

  return (
    <Container>
      <h2>Product Detail</h2>
      <dl>
        <dt>이름</dt>
        <dd>{product.name}</dd>
        <dt>카테고리</dt>
        <dd>{product.category.name}</dd>
        <dt>이미지</dt>
        <dd>
          {product.images.map((image) => (
            <img
              key={image.url}
              src={image.url}
              alt="썸네일"
            />
          ))}
        </dd>
        <dt>가격</dt>
        <dd>
          {numberFormat(product.price)}
          원
        </dd>
        <dt>옵션</dt>
        <dd>
          {product.options.map((option) => (
            <div key={option.id}>
              {option.name}
              :
              {' '}
              {option.items.map((item) => item.name).join(', ')}
            </div>
          ))}
        </dd>
        <dt>설명</dt>
        <dd>
          <Description value={product.description} />
        </dd>
        <dt>표시</dt>
        <dd>{product.hidden ? '숨김' : '보임'}</dd>
      </dl>
      <Link to={`/products/${product.id}/edit`}>
        수정
      </Link>
    </Container>
  );
}
```

### `Description` 컴포넌트 구현 

고객 사이트 만들 때 쓴 걸 거의 그대로 재활용

```tsx
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

* key는 utils의 것을 활용 

### `useFetchProduct` hook 생성

여기까지는 특별한 게 없음      
🎯 상품 추가/수정 기능에서 **데이터 구조를 동적으로 조정하기 위한 Store**를 준비하고, 그 다음에 **화면 꾸미기**

### 테스트 코드 작성 

스토어 구현에 앞서 테스트 코드 작성  

💡 불안할수록 구체적으로 작성할 것 

<details>
<summary>테스트 코드</summary>

```ts
import ProductFormStore from './ProductFormStore';

import fixtures from '../../fixtures';

const createProduct = jest.fn();
const updateProduct = jest.fn();

jest.mock('../services/ApiService', () => ({
    get apiService() {
        return {
            createProduct,
            updateProduct,
        };
    },
}));

const context = describe;

describe('ProductFormStore', () => {
    let store: ProductFormStore;

    beforeEach(() => {
        jest.clearAllMocks();

        store = new ProductFormStore();
    });

    describe('toggleHidden', () => {
        it('changes “hidden” to true and false alternately', () => {
            expect(store.hidden).toBeFalsy();

            store.toggleHidden();

            expect(store.hidden).toBeTruthy();

            store.toggleHidden();

            expect(store.hidden).toBeFalsy();
        });
    });

    describe('create', () => {
        const [category] = fixtures.categories;

        beforeEach(() => {
            store.reset();

            store.changeCategory(category);
            store.changeName('New Product');
            store.changePrice('123400');
            store.changeDescription('What is this?');

            store.changeImageUrl(0, 'http://example.com/images/01.jpg');
            store.addImage();
            store.changeImageUrl(1, 'http://example.com/images/02.jpg');
            store.removeImage(1);

            expect(store.images).toHaveLength(1);

            store.addOption();
            store.addOption();
            store.addOption();
            store.removeOption(2);
            store.changeOptionName(0, 'Color');
            store.changeOptionName(1, 'Size');

            expect(store.options).toHaveLength(2);

            store.addOptionItem(0);
            store.addOptionItem(0);
            // 숫자만 연속되는 부분이  혼란스러우면 object를 이용해 Named Parameter처럼 꾸며주자.
            store.removeOptionItem(0, 2);
            store.changeOptionItemName(0, 0, 'Black');
            store.changeOptionItemName(0, 1, 'White');

            expect(store.options[0].items).toHaveLength(2);

            store.changeOptionItemName(1, 0, 'Free');

            expect(store.valid).toBeTruthy();

            store.toggleHidden();
        });

        context('when API responds with success', () => {
            it('sets done is true', async () => {
                await store.create();

                expect(createProduct).toBeCalled();

                expect(store.done).toBeTruthy();
                expect(store.error).toBeFalsy();
            });
        });

        context('when API responds with error', () => {
            beforeEach(() => {
                createProduct.mockRejectedValue(Error('Create Product API error!'));
            });

            it('sets error is true', async () => {
                await store.create();

                expect(createProduct).toBeCalled();

                expect(store.done).toBeFalsy();
                expect(store.error).toBeTruthy();
            });
        });
    });

    describe('update', () => {
        const [product] = fixtures.products;

        beforeEach(() => {
            store.setProduct(JSON.parse(JSON.stringify(product)));

            store.changeName('New Name');
        });

        context('when API responds with success', () => {
            it('sets done is true', async () => {
                await store.update();

                expect(updateProduct).toBeCalled();

                expect(store.done).toBeTruthy();
                expect(store.error).toBeFalsy();
            });
        });

        context('when API responds with error', () => {
            beforeEach(() => {
                updateProduct.mockRejectedValue(Error('Update Product API error!'));
            });

            it('sets error is true', async () => {
                await store.update();

                expect(updateProduct).toBeCalled();

                expect(store.done).toBeFalsy();
                expect(store.error).toBeTruthy();
            });
        });
    });
});
```

</details>

### ProductFormStore 생성 

상품 추가/수정에서 공통으로 사용할 스토어를 생성

> 💡 원하는 상황을 먼저 테스트 코드로 묘사 → 테스트가 통과하도록 구현 해보기  
> 단 하나의 테스트만 작성해야 한다면? 스토어 등의 **비즈니스 로직**

* `Product`가 `Option`을 갖고, `Option`은 `OptionItem`을 갖는 구조   
  → 깊이 들어간 배열에 값을 추가하거나 삭제하는 게 쉽지 않음 
* 유틸리티 함수 append, remove, update를 `src/utils/index.ts` 파일에 추가

```tsx
export function append<T>(items: T[], item: T) {
  return [...items, item];
}

export function remove<T>(items: T[], index: number) {
  return [
    ...items.slice(0, index),
    ...items.slice(index + 1),
  ];
}

export function update<T>(items: T[], index: number, f: (value: T) => T) {
  return items.map((item, i) => (i === index ? f(item) : item));
}
```

### `useProductFormStore` hook 생성

* hooks/ProductFormStore.ts

ProductFormStore를 쉽게 쓰기 위한 hook 

```tsx
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import ProductFormStore from '../stores/ProductFormStore';

export default function useProductFormStore() {
  const store = container.resolve(ProductFormStore);
  return useStore(store);
}
```

<br>

## 3. 상품 추가

### `ProductNewPage` 생성  

관리자 사이트를 만들면서 쓴 방식보다는 고객 사이트를 만들 때 쓴 방식에 가까움   
💡 **SWR**을 쓰기 때문에 **API 호출이 성공하면 캐시를 초기화**해야 한다는 걸 잊지 말 것

```tsx
export default function ProductNewPage() {
  const navigate = useNavigate();

  const { refresh } = useFetchProducts();
  const { categories } = useFetchCategories();

  const [, store] = useProductFormStore();

  useEffect(() => {
    if (!categories.length) {
      return;
    }

    store.reset();
    store.changeCategory(categories[0]);
  }, [store, categories]);

  const handleComplete = () => {
    refresh();
    navigate('/products');
  };

  if (!categories.length) {
    return null;
  }

  return (
    <ProductNewForm
      categories={categories}
      onComplete={handleComplete}
    />
  );
}
```

### `ProductNewForm` 컴포넌트 구현

고객 사이트와 비슷  
복잡한 로직은 컴포넌트 분리 

```tsx
type ProductNewFormProps = {
  categories: Category[];
  onComplete: () => void;
}

export default function ProductNewForm({
  categories, onComplete,
}: ProductNewFormProps) {
  const [{
    category, images, name, price, options, description, valid, error, done,
  }, store] = useProductFormStore();

  const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    await store.create();
  };

  useEffect(() => {
    if (done) {
      onComplete();
    }
  }, [done]);

  return (
    <Container>
      <h2>New Product</h2>
      <form onSubmit={handleSubmit}>
        <ComboBox
          label="카테고리"
          selectedItem={category}
          items={categories}
          itemToId={(item) => item?.id || ''}
          itemToText={(item) => item?.name || ''}
          onChange={(value) => value && store.changeCategory(value)}
        />
        <Images images={images} store={store} />
        <TextBox
          label="상품명"
          value={name}
          onChange={(value) => store.changeName(value)}
        />
        <TextBox
          type="number"
          label="가격"
          value={price.toString()}
          onChange={(value) => store.changePrice(value)}
        />
        <Options options={options} store={store} />
        <TextBox
          label="설명"
          value={description}
          onChange={(value) => store.changeDescription(value)}
          multiline
        />
        <Button type="submit" disabled={!valid} leftPad>
          등록
        </Button>
        {error && (
          <p>상품 등록 실패</p>
        )}
      </form>
    </Container>
  );
}
```

### `Images` 컴포넌트 구현

* 이미지 삭제
* 이미지 추가

### `Options` 컴포넌트 구현

깊이가 더 깊은 옵션도 다뤄야 하기 때문에 `Options` 컴포넌트를 생성 

* 옵션 삭제
* 옵션 추가

```tsx
type OptionsProps = {
  options: ProductOption[];
  store: Readonly<ProductFormStore>;
}

export default function Options({ options, store }: OptionsProps) {
  return (
    <ul>
      {options.map((option, index) => (
        <li key={key(option.id ?? '', index)}>
          <TextBox
            label={`옵션 #${index + 1}`}
            value={option.name}
            onChange={(value) => store.changeOptionName(index, value)}
          />
          <Button onClick={() => store.removeOption(index)} leftPad>
            옵션 삭제
          </Button>
          <OptionItems
            optionIndex={index}
            items={option.items}
            store={store}
          />
        </li>
      ))}
      <li>
        <Button onClick={() => store.addOption()} leftPad>
          옵션 추가
        </Button>
      </li>
    </ul>
  );
}
```

### `OptionItems` 컴포넌트 구현 

* 아이템 삭제
* 아이템 추가

```tsx
type OptionItemsProps = {
  optionIndex: number;
  items: ProductOptionItem[];
  store: Readonly<ProductFormStore>;
}

export default function OptionItems({
  optionIndex, items, store,
}: OptionItemsProps) {
  return (
    <Container>
      <ol>
        {items.map((item, index) => (
          <li key={key(item.id ?? '', index)}>
            <TextBox
              label={`옵션 아이템 #${optionIndex + 1}-${index + 1}`}
              value={item.name}
              onChange={(value) => (
                store.changeOptionItemName(optionIndex, index, value)
              )}
            />
            <Button onClick={() => store.removeOptionItem(optionIndex, index)}>
              아이템 삭제
            </Button>
          </li>
        ))}
        <li>
          <Button onClick={() => store.addOptionItem(optionIndex)}>
            아이템 추가
          </Button>
        </li>
      </ol>
    </Container>
  );
}
```

<br>

## 4. 상품 수정

수정은 데이터를 가지고와서 처리해야 하기 때문에 다른 것보다 좀 더 복잡함  

### `ProductEditPage` 생성 

* `ProductNewPage`와 유사
* useFetchProduct에서 얻어오기 

```tsx
export default function ProductEditPage() {
  const navigate = useNavigate();

  const params = useParams();
  const productId = String(params.id);

  const { product, refresh } = useFetchProduct({ productId });
  const { categories } = useFetchCategories();

  const [, store] = useProductFormStore();

  useEffect(() => {
    if (!product) {
      return;
    }

    store.setProduct(product);
  }, [store, product]);

  const handleComplete = () => {
    refresh();
    navigate(`/products/${productId}`);
  };

  if (!product || !categories.length) {
    return null;
  }

  return (
    <ProductEditForm
      categories={categories}
      onComplete={handleComplete}
    />
  );
}
```

### `ProductEditForm` 컴포넌트 구현 

* `ProductNewForm` 컴포넌트와 유사
* `Images`, `Options` 등은 전부 그대로 재사용
* 개발 중일 때 hidden 사용 
* `CheckBox` 컴포넌트는 이번에 처음 등장


> 🔎 **팁**
> 
> API를 백엔드 개발자와 같이 만드는 것이 중요   
> 문제점이 있으면 서로 소통해야 함 
