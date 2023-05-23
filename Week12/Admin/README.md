# 1. 관리자 웹 사이트 개발 시작

## 1. 기능

> 📌 기본 중의 기본. 원래는 훨씬 많은 기능이 필요   
> 로그인/로그아웃의 경우 관리자만 가능하도록 구현  
> 상품 관리를 제외하고는 단순한 CRUD 

- 로그인
- 로그아웃
- 회원 관리
    - 회원 목록
- 카테고리 관리
    - 카테고리 목록
    - 카테고리 등록
    - 카테고리 수정
- 주문 관리
    - 주문 목록
    - 주문 상세
    - 주문 상태 변경
- 상품 관리
    - 상품 목록
    - 상품 상세
    - 상품 등록
    - 상품 수정

<br>

## 2. 화면

> 📌 실제로는 주문이 가장 복잡 

1. 홈(대시보드 등 추가 가능) - `/`
2. 로그인 - `/login`
3. 회원 목록 - `/users`
4. 카테고리 목록 - `/categories`
5. 카테고리 등록 - `/categories/new`
6. 카테고리 수정 - `/categories/{id}/edit`
7. 상품 목록 - `/products`
8. 상품 상세 - `/products/{id}`
9. 상품 등록 - `/products/new`
10. 상품 수정 - `/products/{id}/edit`
11. 주문 목록 - `/orders`
12. 주문 상세 - `/orders/{id}`
13. 주문 상태 변경 - `/orders/{id}/edit`
    - 주문 상태 변경을 넘어서, 배송지 수정이나 송장 번호 입력 등을 해볼 수 있음
    - 제품화하려면 해볼 수 있는 게 아니라, **필수**

<br>

## 3. 준비

📌 기존에 작업한 코드를 재활용하면서 상품 등록/수정을 제외한 대부분을 단순 CRUD로 구성할 예정

* 포트 번호 변경 8080 → 8000 
* 단순 작업을 위해 SWR과 React Hook Form을 사용

### SWR

[SWR](https://swr.vercel.app/ko)

```bash
npm i swr
```

F/E에서 **상태 관리**를 아주 적극적으로 하는 흐름이 있다면, 
반대로 F/E에선 **캐시**에 집중하고 **백엔드와의 동기화**에 집중하는 흐름이 존재

* **백엔드**에서 상태를 처리하고 있으므로 그 정보를 잘 가져오자
* 너무 많이 요청하면 서버에 부하가 크기 때문에 **캐시**를 잘 활용하자  

여기서는 B/E에 의존적인 아주 단순한 CRUD 애플리케이션을 구축하기 위해 **SWR** 사용   
좀 더 복잡한 걸 다루고 싶다면 **TanStack Query(React Query)** 사용

### React Hook Form

[React Hook Form](https://react-hook-form.com/)

```bash
npm i react-hook-form
```

[Uncontrolled Component](https://ko.legacy.reactjs.org/docs/uncontrolled-components.html)
 
단순한 CRUD 애플리케이션을 구축한다면 **Uncontrolled Component**를 사용하거나, 
그에 준하는 편의성을 제공하는 도구를 활용할 수 있음   

React Hook Form은 이 모두를 지원  
특히 Uncontrolled 방식에 집중해서 활용하면 리렌더링이 줄어들어 커다란 **폼의 성능** 문제에서 유리  
스토어를 따로 만들지 않고 진행  

### Types

관리자 API에 맞는 타입을 구성

```tsx
export type User = {
  id: string;
  name: string;
  email: string;
  role: string;
}

export type Category = {
  id: string;
  name: string;
  hidden?: boolean;
}

export type Image = {
  url: string;
}

export type ProductSummary = {
  id: string;
  category: Category;
  thumbnail: Image;
  name: string;
  price: number;
  hidden: boolean;
}

export type ProductOptionItem = {
  id: string;
  name: string;
  deleted: boolean;
};

export type ProductOption = {
  id: string;
  name: string;
  items: ProductOptionItem[];
  deleted: boolean;
};

export type ProductDetail = {
  id: string;
  category: Category;
  images: Image[];
  name: string;
  price: number;
  options: ProductOption[];
  description: string;
  hidden: boolean;
}

export type OrderOptionItem = {
  name: string;
};

export type OrderOption = {
  name: string;
  item: OrderOptionItem;
};

export type LineItem = {
  id: string;
  product: {
    id: string;
    name: string;
  };
  options: OrderOption[];
  unitPrice: number;
  quantity: number;
  totalPrice: number;
}

export type OrderSummary = {
  id: string;
  orderer: User;
  title: string;
  totalPrice: number;
  status: string;
  orderedAt: string;
}

type Receiver = {
  name: string;
  address1: string;
  address2: string;
  postalCode: string;
  phoneNumber: string;
}

type Payment = {
  merchantId: string;
  transactionId: string;
}

export type OrderDetail = {
  id: string;
  orderer: User;
  title: string;
  lineItems: LineItem[];
  totalPrice: number;
  receiver: Receiver;
  payment: Payment;
  status: string;
  orderedAt: string;
}
```

### ApiService

여기선 관리자 API에 맞게 ApiService가 준비되었다고 가정   
CRUD 중 Read에 관한 건 대부분 SWR를 사용하기 때문에 대부분은 CUD,
즉 Command(또는 Mutation)에 대한 코드만 존재 
