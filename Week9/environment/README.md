# 1. 개발하기 전 준비

## 1. 용어

> 📌 팀이 계속 가지고 가야할 것. 필요하면 업데이트하고 잘 정리해두자   

- Product : 상품
    - Summary : 상품에 대한 요약 정보
    - Detail : 상품에 대한 상세 정보
    - Image : 상품 이미지
    - Option : 상품에 대한 상세 옵션 종류 (색상, 크기 등)
        - OptionItem : 옵션에 대한 상세 옵션 값 (옵션이 색상이라면, OptionItem은 Blue, Red 같은 값을 의미함)
- Category : 상품에 대한 분류
- Cart : 장바구니
    - LineItem : 장바구니에 담긴 것 (상품, 옵션, 수량 등을 동시에 다룸, 추상화된 개념)
        - 여기서도 Option과 OptionItem을 사용(같은 대상을 가리키지만, 카트에서는 특정한 값을 드러내기 위해 사용)
        - 용어는 동일하지만 Product와 다른 구성을 갖기 때문에, 여기서는 Product와 Order라는 접두어를 활용
        - 시스템을 분리할 수 있다면, 근본적으로 나누는 걸 추천(상품 정보 확인 / 장바구니 / 주문)
- Order : 주문
    - 여기서도 동일한 LineItem 활용
- User : 사용자

<br>

## 2. 기능

> ✅ 매출이 바로 일어날 수 있는 3가지   
> 🎯 어떤 순서로 기능을 만들 것인지는 비즈니스 우선 순위에 따라 결정  

1. 상품 목록 확인 ✅
2. 상품 상세 정보 확인 ✅
3. 장바구니에 상품 담기 ✅
4. 주문하기 → 배송지 입력, 결제
5. 주문 목록 확인
6. 주문 상세 확인
7. 로그인
8. 회원 가입

<br>

## 3. 화면

1. 홈 페이지 - `/`
2. 상품 목록 페이지 - `/products`
3. 상품 상세 페이지 - `/products/{id}`
4. 장바구니 페이지 - `/cart`
5. 주문 페이지 - `/order`
6. 주문 완료 페이지 - `/order/complete`
7. 주문 목록 페이지 - `/orders`
8. 주문 상세 페이지 - `/orders/{id}`
9. 로그인 페이지 - `/login`
10. 회원 가입 페이지 - `/signup`
11. 회원 가입 완료 페이지 - `/signup/complete`

<br>

## 4. 개발 환경 세팅

> [🔗 실습 링크 : Styles, Routes 설정 세팅](https://github.com/ShinjungOh/2023-learn-react/commit/d7a15c0fedf570e6bcfb5e4bff3dbc5068b49a4c)  
> [🔗 실습 링크 : Test Helper, Types, MSW 세팅](https://github.com/ShinjungOh/2023-learn-react/commit/c6a043bf8a969849ffa6219c4a058873c024f2d9)

### REST API

백엔드와의 협업   
API 스펙에 맞춰 진행

### 기본 세팅

[기본 세팅](https://github.com/ahastudio/til/blob/main/react/20230205-setup-react-project.md)  
[CodeceptJS 세팅](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-사용)  

### 추가로 설치할 라이브러리

1. [React Router](https://github.com/remix-run/react-router)
2. [styled-components](https://github.com/styled-components/styled-components)
3. [styled-reset](https://github.com/zacanger/styled-reset)
4. [usehooks-ts](https://github.com/juliencrn/usehooks-ts)
5. [Axios](https://github.com/axios/axios) : REST API 사용을 위한 HTTP 클라이언트, fetch보다 간편 
6. [tsyringe](https://github.com/microsoft/tsyringe)
7. [reflect-metadata](https://github.com/rbuckton/reflect-metadata) : tsyringe를 위해 사용
8. [usestore-ts](https://github.com/seed2whale/usestore-ts)
9. [jest-dom](https://github.com/testing-library/jest-dom) : React Testing Library에서 활용할 수 있는 DOM 확인용 Matcher 모음
10. [MSW](https://github.com/mswjs/msw)

<br>

## 5. E2E Test

CodeceptJS로 E2E 테스트 준비하고, 여기 있는 기능 테스트를 모두 통과시키는 걸 목표로 함 

1. 상품 목록
    1. 모든 상품 보기
    2. 특정 카테고리의 상품 보기
2. 상품 상세
3. 장바구니
    1. 장바구니가 비어있는 경우
    2. 장바구니에 상품을 담은 경우
4. 추후 추가…

<br>

## 6. Styles

> [🔗 참고 자료](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week8/globalstyle-and-theme)

styles-components를 사용하기 위해 **defaultTheme**과 **GlobalStyles**를 준비  
기본 코드는 기존과 크게 다르지 않음

<br>

## 7. Routes

> [🔗 참고 자료](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week7/router)

React Router로 여러 페이지를 표현하기 위해 **routes**와 **Layout**을 준비  
기본 코드는 기존과 크게 다르지 않음

<br>

## 8. Test Helper

테스트 코드에서 styled-components의 Theme과 React Router의 Link 등을
사용할 때 문제가 발생하지 않도록(Context API - provider 필요),
React Testing Library의 render를 한번 감싼 테스트용 헬퍼 함수를 준비

* 유틸리티 함수, 헬퍼 함수 등으로 불림  

```tsx
// src/test-helpers.tsx 

import { render as originalRender } from '@testing-library/react';

import React from 'react';

import { MemoryRouter } from 'react-router-dom';

import { ThemeProvider } from 'styled-components';

import defaultTheme from './styles/defaultTheme';

export function render(element: React.ReactElement) {
  return originalRender((
    <MemoryRouter initialEntries={['/']}>
      <ThemeProvider theme={defaultTheme}>
        {element}
      </ThemeProvider>
    </MemoryRouter>
  ));
}
```

<br>

## 9. Types

기본적인 타입을 준비  
특별한 경우가 아니라면 미리 정한 용어집과 REST API 스펙에 맞추게 됨  
타입을 잘 잡아주면 **자동 완성**이 매우 편리 

```tsx
// src/types.ts

export type Category = {
  id: string;
  name: string;
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
}

export type ProductOptionItem = {
  id: string;
  name: string;
};

export type ProductOption = {
  id: string;
  name: string;
  items: ProductOptionItem[];
};

export type ProductDetail = {
  id: string;
  category: Category;
  images: Image[];
  name: string;
  price: number;
  options: ProductOption[];
  description: string;
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

export type Cart = {
  lineItems: LineItem[];
  totalPrice: number;
}
```

<br>

## 10. MSW 세팅

> [🔗 참고 자료](https://shinjungohs-dev-road.gitbook.io/megaptera-frontend/undefined/week5/msw)

REST API 스펙에 맞춰서 MSW 핸들러를 준비

```tsx
// src/mocks/handlers.ts

import { rest } from 'msw';

import { ProductSummary } from '../types';

import fixtures from '../../fixtures';

const BASE_URL = 'https://shop-demo-api-01.fly.dev';

const productSummaries: ProductSummary[] = fixtures.products
  .map((product) => ({
    id: product.id,
    category: product.category,
    thumbnail: { url: product.images[0].url },
    name: product.name,
    price: product.price,
  }));

const handlers = [
  rest.get(`${BASE_URL}/categories`, (req, res, ctx) => (
    res(ctx.json({ categories: fixtures.categories }))
  )),
  rest.get(`${BASE_URL}/products`, (req, res, ctx) => (
    res(ctx.json({ products: productSummaries }))
  )),
  rest.get(`${BASE_URL}/products/:id`, (req, res, ctx) => {
    const product = fixtures.products.find((i) => i.id === req.params.id);
    if (!product) {
      return res(ctx.status(404));
    }
    return res(ctx.json(product));
  }),
  rest.get(`${BASE_URL}/cart`, (req, res, ctx) => (
    res(ctx.json(fixtures.cart))
  )),
  rest.post(`${BASE_URL}/cart/line-items`, (req, res, ctx) => (
    res(ctx.status(201))
  )),
];

export default handlers;
```

> ✍️ **조각글**
> 
> 강의를 따라 실습 세팅하는데만 해도 시간이 오래 걸렸다. 
> 강의 분량이 역대급으로 많아서 과제는 둘째치고 노트정리를 제 시간에 끝마칠 수 있을지 걱정이다.
> 이번 장은 용어집이나 환경 세팅 부분이기 때문에 자주 찾아볼 것 같다. 
> 집중해서 본격적인 구현도 잘 따라가야겠다. 
