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
