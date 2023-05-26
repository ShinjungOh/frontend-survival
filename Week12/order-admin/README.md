# 4. 주문 관리

## 1. 주문 목록

🎯 주문 관리 기능을 간단히 다뤄보기

실제 쇼핑몰을 만들 때는 여러가지 상태 정보가 필요하기 때문에 주문 관리가 제일 복잡(특히 백엔드)

* 송장 번호
* 취소시 환불
* 부분 취소

### `OrderListPage` 구현 

주문 목록을 확인하는 페이지    
조금 양이 많은 사용자 또는 카테고리 목록 

```tsx
export default function OrderListPage() {
  const { orders, loading, error } = useFetchOrders();

  // loading, error 처리 

  return (
    <Container>
      <h2>Orders</h2>
      <table>
        <thead>
          <tr>
            <th>주문일</th>
            <th>주문자</th>
            <th>상품</th>
            <th>총 가격</th>
            <th>상태</th>
            <th>행동</th>
          </tr>
        </thead>
        <tbody>
          {orders.map((order) => (
            <tr key={order.id}>
              <td>{order.orderedAt}</td>
              <td>{order.orderer.name}</td>
              <td>{order.title}</td>
              <td>
                {numberFormat(order.totalPrice)}
                원
              </td>
              <td>{STATUS_MESSAGES[order.status]}</td> 
              <td>
                <Link to={`/orders/${order.id}`}>
                  자세히
                </Link>
                <Link to={`/orders/${order.id}/edit`}>
                  상태 변경
                </Link>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </Container>
  );
}
```

* STATUS_MESSAGES : 언어를 서버에서 변경해 줄 수도 있지만 다국어 지원 등이 불편하기 때문에 이렇게 처리
* 다국어 지원을 위한 라이브러리도 존재 

### `contants.ts` 파일에서 상수 관리 

주문 상태를 어떻게 표시할지 상수만 모은 `contants.ts` 파일에서 정의

```tsx
export const STATUS_MESSAGES: Record<string, string> = {
  paid: '결제 완료',
  ready: '배송 준비',
  shipping: '배송 중',
  complete: '배송 완료',
  canceled: '취소',
};
```

* 배송 준비 : 택배를 보내려고 포장하는 상태 
* 배송 중 : 택배 기사에게 전달 후 
* 취소 : 관리자가 실수했을 때 처리
  * 어떤 일이 있었는지 로그가 필요 

### `useFetchOrders` hook 생성 

```tsx
import useFetch from './useFetch';

import { OrderSummary } from '../types';

export default function useFetchOrders() {
  const { data, error, loading } = useFetch<{
    orders: OrderSummary[];
  }>('/orders');

  return {
    orders: data?.orders ?? [],
    error,
    loading,
  };
}
```

<br>

## 2. 주문 상세

🎯 주문 상세 정보 확인

### OrderDetailPage 구현 

```tsx
export default function OrderDetailPage() {
  const params = useParams();

  const { order, loading, error } = useFetchOrder({
    orderId: String(params.id),
  });

  // loading, error 처리
    
  return (
    <Container>
      <h2>Order Detail</h2>
      <dl>
        <dt>주문일시</dt>
        <dd>{order.orderedAt}</dd>
        <dt>주문자</dt>
        <dd>{order.orderer.name}</dd>
        <dt>상품</dt>
        <dd>
          <ul>
            {order.lineItems.map((lineItem) => (
              <li key={lineItem.id}>
                {lineItem.product.name}
                <Options options={lineItem.options} />
              </li>
            ))}
          </ul>
        </dd>
        <dt>총 가격</dt>
        <dd>
          {numberFormat(order.totalPrice)}
          원
        </dd>
        <dt>배송 정보</dt>
        <dd>
          <p>받는 사람:{order.receiver.name}</p>
          <p>연락처:{order.receiver.phoneNumber}</p>
          <p>
            {order.receiver.address1}
            {order.receiver.address2}
            (우편번호: {order.receiver.postalCode})
          </p>
        </dd>
        <dt>결제 정보</dt>
        <dd>
          <p>주문번호: {order.payment.merchantId}</p>
          <p>결제고유번호: {order.payment.transactionId}</p>
        </dd>
        <dt>상태</dt>
        <dd>{STATUS_MESSAGES[order.status]}</dd>
      </dl>
      <Link to={`/orders/${order.id}/edit`}>
        상태 변경
      </Link>
    </Container>
  );
}
```

* STATUS_MESSAGES 사용 
* `<Link>`로 처리된 `상태 변경`은 버튼으로 구현해도 좋음  
* `상태 변경`과 `주문 목록` 버튼 구현하기 
* `상태`에는 **송장 번호**가 같이 나오면 좋음(배송 추적 가능) 

### `useFetchOrder` hook 생성 

기존과 비슷하게 작성 

* orderId를 인자로 받음 

<br>

## 3. 주문 상태 변경

🎯 단순하게 주문 상태만 변경해보기

> 👩🏻‍💼 고객 지원 업무(CS)를 위해서는 주문을 자세히 다룰 수 있는 기능이 필요 
> 
> * 주소를 잘못 입력했을 경우
> * 이사 간 주소로 변경할 경우 
> * 배송 시작했을 경우 송장 번호 추가 (배송 시작 버튼을 누르면 송장 번호를 입력)
>  * 이후 배송 중 상태로 변경

필요에 따라 B/E 개발자와 논의해 계속해서 확장해 나갈 것  
⇒ 단순 CRUD를 넘어서 비즈니스에 대한 이해가 필요한 부분 

### `OrderEditPage` 구현 

```tsx
export default function OrderEditPage() {
  const params = useParams();

  const orderId = String(params.id);

  const navigate = useNavigate();

  const {
    order, loading, error, updateOrder,
  } = useFetchOrder({
    orderId,
  });

  type FormValues = {
    status: string;
  };

  const { handleSubmit, control } = useForm<FormValues>();

  const onSubmit = async (data: FormValues) => {
    await updateOrder({
      status: data.status,
    });
    navigate(`/orders/${orderId}`);
  };
  
  // loading, error 처리

  return (
    <Container>
      <h2>Order Status Transition</h2>
      <dl>
        <dt>주문일시</dt>
        <dd>{order.orderedAt}</dd>
        <dt>주문자</dt>
        <dd>{order.orderer.name}</dd>
        <dt>상품</dt>
        <dd>{order.title}</dd>
        <dt>총 가격</dt>
        <dd>
          {numberFormat(order.totalPrice)}
          원
        </dd>
      </dl>
      <form onSubmit={handleSubmit(onSubmit)}>
        <Controller
          control={control}
          name="status"
          defaultValue={order.status}
          render={({ field: { onChange, value } }) => (
            <div>
              <label htmlFor="input-status">상태</label>
              <select
                id="input-status"
                value={value}
                onChange={onChange}
              >
                {Object.keys(STATUS_MESSAGES).map((status) => (
                  <option key={status} value={status}>
                    {STATUS_MESSAGES[status]}
                  </option>
                ))}
              </select>
            </div>
          )}
        />
        <Button type="submit">
          변경
        </Button>
      </form>
    </Container>
  );
}
```

### `useFetchOrder` hook에 updateOrder 추가

```tsx
export default function useFetchOrder({ orderId }: {
  orderId: string;
}) {
  const {
    data, error, loading, mutate,
  } = useFetch<OrderDetail>(`/orders/${orderId}`);

  return {
    order: data,
    error,
    loading,
    async updateOrder({ status }: {
      status: string;
    }) {
      await apiService.updateOrder({ orderId, status });

      mutate();
    },
  };
}
```
