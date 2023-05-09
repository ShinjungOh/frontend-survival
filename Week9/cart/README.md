# 4. 장바구니 보기

## 1. 장바구니 데이터 받아오기

> [🔗 실습 링크 : 장바구니 보기 CartStore, useFetchCart 구현](https://github.com/ShinjungOh/2023-learn-react/commit/3ab76dbfa50708e0f7568817828d774b792c315c)

🎯 **장바구니에 상품 담기** 기능을 만들기 전에, 비교적 쉬운 **장바구니 보기** 작업을 먼저 해보기 

### `CartPage.tsx` 작성

```tsx
const {cart} = useFetchCart();
```

### useFetchCart hook 생성

* src/hooks
* 원래는 API을 호출할 때 **엑세스 토큰**을 넘겨주어, '내 카트'를 호출해야 함
* 지금은 모두가 같은 카트를 이용하도록 구현됨
* 이후 회원가입, 로그인 구현 시 처리할 예정  

```tsx
export default function useFetchCart() {
  const store = container.resolve(CartStore);

  const [{ cart }] = useStore(store);

  useEffectOnce(() => {
    store.fetchCart();
  });

  return { cart };
}
```

### CartStore hook 생성 

* src/stores

#### 구현 내용

1. 타입 지정
   * ProductDetail처럼 Null Object를 만들어서 쓰면 더 좋음

2. API Service에 fetchCart 추가
   * ApiService에서 fetchCart 구현

3. setCart @Action() 추가 

```
async fetchCart(): Promise<Cart> {
  const { data } = await this.instance.get('/cart');
  return data;
}
```

<br>

## 2. 장바구니 보기 페이지 UI 구현하기

> [🔗 실습 링크 : 장바구니 보기 페이지 UI 구현](https://github.com/ShinjungOh/2023-learn-react/commit/e3cff1d21d2a163596066b382245ad53caa2523b)

Cart 타입과 Cart 컴포넌트의 이름이 겹치는 이슈 

1. Cart 타입을 가져올 때 as CartType을 써서 타입의 이름을 교체 
2. Cart 컴포넌트를 CartView 등의 다른 이름으로 교체 ✅

### `CartView.tsx` 컴포넌트 구현

* src/components/cart/CartView.tsx

```tsx
const Container = styled.div`
  table {
    width: 100%;
  }

  th, td {
    padding: .5rem;
    text-align: left;
  }
`;

type CartViewProps = {
  cart: Cart;
};

export default function CartView({ cart }: CartViewProps) {
  if (!cart.lineItems.length) {
    return (
      <p>장바구니가 비었습니다</p>
    );
  }

  return (
    <Container>
      <table>
        <thead>
          <tr>
            <th>제품</th>
            <th>단가</th>
            <th>수량</th>
            <th>금액</th>
          </tr>
        </thead>
        <tbody>
          {cart.lineItems.map((lineItem) => (
            <LineItemView
              key={lineItem.id}
              lineItem={lineItem}
            />
          ))}
        </tbody>
        <tfoot>
          <tr>
            <th colSpan={3}>
              합계
            </th>
            <td>
              {numberFormat(cart.totalPrice)}
              원
            </td>
          </tr>
        </tfoot>
      </table>
    </Container>
  );
}
```

### LineItemView 컴포넌트 생성

이름 중복 이슈로 인해 LineItem도 LineItemView라는 이름으로 생성 

```tsx
      <td>
        <Link to={`/products/${lineItem.product.id}`}>
          {lineItem.product.name}
        </Link>
        <Options options={lineItem.options} />
      </td>
```

* 해당 상품으로 돌아갈 수 있는 Link 추가
* 선택한 옵션을 볼 수 있는 Options 컴포넌트 생성 

### Options 컴포넌트 생성

```tsx
const Container = styled.div`
  margin-top: .5rem;
  font-size: 1.4rem;
`;

type OptionsProps = {
  options: OrderOption[];
}

export default function Options({ options }: OptionsProps) {
  if (!options.length) {
    return null;
  }

  const text = options
    .map((option) => `${option.name}: ${option.item.name}`)
    .join(', ');

  return (
    <Container>
      ({text})
    </Container>
  );
}
```

### cURL을 이용해서 임의로 장바구니에 상품 담기

[cURL](https://ko.wikipedia.org/wiki/CURL)  

서버와 통신할 수 있는 커맨드 명령어 툴

```bash
curl -X POST https://shop-demo-api-01.fly.dev/cart/line-items \
  -H "Content-Type: application/json" \
  --data '
    {
      "productId": "0BV000PRO0001",
      "options": [
        {
          "id": "0BV000OPT0001",
          "itemId": "0BV000ITEM001"
        },
        {
          "id": "0BV000OPT0002",
          "itemId": "0BV000ITEM005"
        }
      ],
      "quantity": 1
    }
  '
```

<br>

## 3. 테스트 작성하기 

### backdoor

데이터베이스를 초기화해서 다시 테스트할 수 있도록 함  
백엔드 개발자와 얘기해서 만든 것 

🚨 테스트 환경애서만 사용해야 함, 실제 서비스에선 절대 나가면 안됨 


