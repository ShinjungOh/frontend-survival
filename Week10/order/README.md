# 4. 주문 목록 & 주문 상세

## 1. 주문 목록 확인

> 🛒 장바구니와 비슷하지만, 주문 목록은 개인화된 내용  

### 주문 목록 API에 맞춘 types 추가

```tsx
export type OrderSummary = {
  id: string;
  title: string;
  totalPrice: number;
  status: string;
  orderedAt: string;
}
```

### `ApiService`에 fetchOrders 메소드 추가

```tsx
async fetchOrders(): Promise<OrderSummary[]> {
  const { data } = await this.instance.get('/orders');
  const { orders } = data;
  return orders;
}
```

### `OrderListStore` 추가

```tsx
@singleton()
@Store()
export default class OrderListStore {
  orders: OrderSummary[] = [];

  async fetchOrders() {
    this.setOrders([]);

    const orders = await apiService.fetchOrders();

    this.setOrders(orders);
  }

  @Action()
  setOrders(orders: OrderSummary[]) {
    this.orders = orders;
  }
}
```

### Hook 생성

### `OrderListPage` 생성

```tsx
export default function OrderListPage() {
  const { orders } = useFetchOrders();

  return (
    <div>
      <h2>주문 목록</h2>
      <Orders orders={orders} />
    </div>
  );
}
```

### `routes`에 페이지 추가

```tsx
{ path: '/orders', element: <OrderListPage /> },
```

### `Header`에 링크 추가

```tsx
<li>
  <Link to="/orders">Orders</Link>
</li>
```

### `Orders` 컴포넌트 작성

```tsx
type OrdersProps = {
  orders: OrderSummary[];
}

export default function Orders({ orders }: OrdersProps) {
  if (!orders.length) {
    return null;
  }

  return (
    <Container>
      <ul>
        {orders.map((order) => (
          <li key={order.id}>
            <Link to={`/orders/${order.id}`}>
              <Order order={order} /> 
            </Link>
          </li>
        ))}
      </ul>
    </Container>
  );
}
```

### `Order` 컴포넌트 작성

```tsx
export default function Order({ order }: OrderProps) {
  return (
    <Container>
      <div>
        주문 일시: {order.orderedAt}
      </div>
      <div>
        주문 코드: {order.id}
      </div>
      <div>
        상품: {order.title}
      </div>
      <div>
        결제 금액: {numberFormat(order.totalPrice)}원
      </div>
    </Container>
  );
}
```

<br>


## 2. 주문 상세 확인

> 🛒 상품 목록/장바구니와 비슷  
> 🛍 디테일은 상품 디테일과 유사  

### 주문 상세 API에 맞춘 types 추가

```tsx
export type OrderDetail = {
  id: string;
  title: string;
  lineItems: LineItem[];
  totalPrice: number;
  status: string;
  orderedAt: string;
}
```

### Null Object 준비

```tsx
export const nullOrderDetail: OrderDetail = {
  id: '',
  title: '',
  status: '',
  lineItems: [],
  totalPrice: 0,
  orderedAt: '',
};
```

### `ApiService`에 fetchOrder 메소드 추가

```tsx
async fetchOrder({ orderId }: {
  orderId: string;
}): Promise<OrderDetail> {
  const { data } = await this.instance.get(`/orders/${orderId}`);
  return data;
}
```

### `OrderDetailStore` 추가

* 중복이 싫을 경우, 보편화하는 방법을 찾아도 됨   

### 주문 상세 정보를 얻는 `useFetchOrder` Hook 생성 

* useFetchProduct 복사해서 사용 

```tsx
export default function useFetchOrder({ orderId }: {
  orderId: string;
}) {
  const store = container.resolve(OrderDetailStore);

  const [{ order, loading, error }] = useStore(store);

  useEffect(() => {
    store.fetchOrder({ orderId });
  }, [store]);

  return { order, loading, error };
}
```

### OrderDetailPage 생성 

* 로딩, 에러 처리

```tsx
export default function OrderDetailPage() {
  const params = useParams();

  const { order, loading, error } = useFetchOrder({
    orderId: String(params.id),
  });

  // 로딩, 에러 로직 추가 
  
  return (
    <Order order={order} />
  );
}
```

### `routes`에 페이지 추가

### Order 컴포넌트 구현 

처리 상태를 보고 싶은 경우 `{order.status}`로 받아올 수 있음 

```tsx
const STATUS_MESSAGE: Record<string, string> = {
    'paid': '결제 완료'
}
```

```tsx
export default function Order({ order }: OrderProps) {
  if (!order.lineItems.length) {
    return null;
  }

  return (
    <Container>
      <dl>
        <dt>주문 일시</dt>
        <dd>{order.orderedAt}</dd>
        <dt>주문 코드</dt>
        <dd>{order.id}</dd>
      </dl>
      <Table
        lineItems={order.lineItems}
        totalPrice={order.totalPrice}
      />
    </Container>
  );
}
```

### `<dl />`

[MDN `<dl>`](https://developer.mozilla.org/ko/docs/Web/HTML/Element/dl)

Description List, 설명 목록

`<dt>`로 표기한 용어와 `<dd>`로 표기한 설명 그룹의 목록을 감싸서 설명 목록을 생성  
주로 용어사전 구현이나 메타데이터(키-값 쌍 목록)를 표시할 때 사용

### UI 구현하기 

`LineItem`을 이용해 상품 목록을 보여주는 부분은 
장바구니 보여줄 때 만든 `LineItemView`와 `Options` 컴포넌트를 재사용

```tsx
export default function CartView({ cart }: CartViewProps) {
  if (!cart.lineItems.length) {
    return (
      <p>장바구니가 비었습니다</p>
    );
  }

  return (
    <Table
      lineItems={cart.lineItems}
      totalPrice={cart.totalPrice}
    />
  );
}
```

### `Table` 컴포넌트 추출

`CartView` 컴포넌트에서 `Table` 컴포넌트를 추출

```tsx
export default function Table({ lineItems, totalPrice }: TableProps) {
  if (!lineItems.length) {
    return null;
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
          {lineItems.map((lineItem) => (
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
              {numberFormat(totalPrice)}
              원
            </td>
          </tr>
        </tfoot>
      </table>
    </Container>
  );
}
```
