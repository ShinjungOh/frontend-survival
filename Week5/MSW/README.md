# 3. MSW

## 1. Service Worker

[Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)

<figure><img src="https://github.com/ShinjungOh/TIL/blob/main/Images/webworker2.png" alt="service_worker" width="100%"></figure>

### 웹 워커 vs 서비스 워커

#### 공통점

* [Web API](https://developer.mozilla.org/ko/docs/Web/API) 중 하나
* 웹 사이트에서 사용할 수 있는 워커
* 둘 다 보조 스레드에서 실행되므로 기본 스레드와 사용자 인터페이스를 차단하지 않고 JavaScript 코드를 실행
* Window 및 Document objects에 대한 액세스 권한이 없으므로, DOM과 직접 상호 작용할 수 없으며 브라우저 API에 대한 액세스가 제한됨

<br>

#### 차이점

|         |        웹 워커         |                         서비스 워커                         |
|:-------:|:-------------------:|:------------------------------------------------------:|
|   수명    | 웹 워커가 속한 탭과 밀접하게 연결 |                          독립적                           |
| 탭 종료 시  |         종료됨         |                    백그라운드에서 계속 실행 가능                    |
|   갯수    |    여러 웹 워커 생성 가능    |                  등록된 범위의 모든 활성 탭을 제어                   |
| 네트워크 요청 |          -          | 네트워크 요청을 가로챔(fetch이벤트를 통해),<br/>백그라운드에서 푸시 API 이벤트를 수신 |

* 웹 워커의 수명은 웹 워커가 속한 탭과 밀접하게 연결되어 있는 반면, 서비스 워커의 수명 주기는 독립적  
  웹 워커가 실행 중인 탭을 닫으면 종료되지만, 서비스 워커는 사이트에 열려 있는 활성 탭이 없는 경우에도 백그라운드에서 계속 실행 가능
* 페이지는 여러 웹 워커를 생성할 수 있지만, 단일 서비스 워커는 등록된 범위의 모든 활성 탭을 제어
* 웹 워커와 달리 서비스 워커를 사용하면 fetch이벤트를 통해 네트워크 요청을 가로채고,  
  백그라운드에서 push이벤트를 통해 Push API 이벤트를 수신

<br>

## 2. MSW(Mock Service Worker)

[MSW](https://mswjs.io/)  
[아샬님의 Mock Service Worker (MSW)](https://github.com/ahastudio/til/blob/main/mock-api/msw.md)

express 보다 조금 불편함  
코드 레벨이 아니라 **네트워크 레벨**에서 가짜 구현(프록시를 이용)  
**오프라인 작업** 등을 지원하기 위한 서비스 워커의 기능을 유용하게 활용한 것  
Node, 브라우저 둘 다 지원

> 🥊 **Express vs MSW**
>
> MSW는 jest의 테스트 환경(Node.js 기반) 외에 웹 브라우저도 지원  
> API 스펙은 나왔지만 아직 구현되지 않은 경우 임시로 사용할 수 있음
> 
> 단순히 임시 서버를 만들 거라면 Express를 쓰는 게 더 낫지만,
> MSW는 테스트 코드도 지원하면서 겸사겸사 웹 브라우저를 지원하는 용도로는 나쁘지 않은 선택

### 사용 방법

[Integrate mocking into Node](https://mswjs.io/docs/getting-started/integrate/node)

#### 1. MSW 패키지 설치

```bash
npm i -D msw
```

#### 2. `server.ts` 파일 생성

src/mocks/server.ts 경로에 생성

```ts
// src/mocks/server.ts

import {setupServer} from 'msw/node';

// Import handlers from './handlers';

const handlers = []; // 임시로 적어둠 

// This configures a request mocking server with the given request handlers.
const server = setupServer(...handlers);

export default server;
```

#### 3. [setupTests.ts](https://mswjs.io/docs/getting-started/integrate/node) 파일 생성

```ts
// src/setupTests.ts

import server from './mocks/server.ts';
// Establish API mocking before all tests.
beforeAll(() => server.listen({onUnhandledRequest: 'error'}));

// Reset any request handlers that we may add during the tests,
// so they don't affect other tests.
afterEach(() => server.resetHandlers());

// Clean up after the tests are finished.
afterAll(() => server.close());
```

* beforeAll : Jest 시작할 때 맨 처음에 실행
    * `onUnhandledRequest: 'error'` : handler를 안 잡았을 때 오류 내도록 설정
* afterEach : 각 테스트가 끝날 때마다 실행
* afterAll : 전부 끝날 때 실행

#### 3. `jest.config.js` 파일의 “setupFilesAfterEnv” 속성에 `setupTests.ts` 파일 추가

```js
// jest.config.js

module.exports = {
    testEnvironment: 'jsdom',
    setupFilesAfterEnv: [
        '@testing-library/jest-dom/extend-expect',
        '<rootDir>/src/setupTests.ts',
    ],
    transform: {
        '^.+\\.(t|j)sx?$': ['@swc/jest', {
            jsc: {
                parser: {
                    syntax: 'typescript',
                    jsx: true,
                    decorators: true,
                },
                transform: {
                    react: {
                        runtime: 'automatic',
                    },
                },
            },
        }],
    },
};
```

<br>

## 3. REST API 모킹하기

[Mocking REST API](https://mswjs.io/docs/getting-started/mocks/rest-api)

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/d2c96d712e3a1d14e276677b18ca9cf889dc4cbb)

> ⚠️ 너무 본격적으로 코딩하면 사실상 백엔드를 개발하게 되니, 이 부분에 주의할 것

### 폴더 구조

```
├── package.json
├── package-lock.json
├── jest.config.js ✅
├── src
│   ├── main.tsx
│   ├── App.tsx
│   ├── App.test.tsx ✅
│   ├── setupTests.ts ✅
│   ├── components 📁
│   ├── hooks 📁
│   ├── mocks 📁
│   │   ├── handlers.ts ✅
│   │   └── server.ts ✅
```

### handlers.ts 파일 작성

`진짜같은 것`이지 `진짜`가 아님  
진짜도 테스트가 필요함 ⇒ E2E 테스트 

```ts
// src/mocks/handlers.ts 

import {rest} from 'msw';
// import fixtures from '../../fixtures';

const BASE_URL = 'http://localhost:3000';

const handlers = [
    rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
        // const { products } = fixtures; // fixtures를 사용해도 됨  
        const products = [
            {
                category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
            },
        ];

        return res(
            ctx.status(200), // 없어도 됨(기본 설정이 200)
            ctx.json({products}), // 위의 요청을 이 형태로 반황 
        );
    }),
];

export default handlers;
```

### App.test.ts 파일 수정

```ts
// App.test.ts

import {render, screen, waitFor} from '@testing-library/react';

import App from './App';

// jest.mock 불필요

test('App', async () => {
    render(<App / >);

    await waitFor(() => {
        screen.getByText('Apple');
    });
});
```

* waitFor : ~ 가 될 때까지 대기
    * 콜백함수의 타입이 Promise로 되어 있어서 async/await 필요

### 주의점 & polyfill 이용하기

```ts
// hooks/useFetchProducts.ts

export default function useFetchProducts() {
    const url = 'http://localhost:3000/products';
    const {data, error} = useFetch<ProductsResult>(url);
    console.log({error});
    if (!data) {
        return [];
    }

    return data.products;
}
```

```
🚨 error: ReferenceError: fetch is not defined
```

fetch는 윈도우에 있는 것  
브라우저에서는 되지만 Node에서는 오류    
Node 최신 버전은 fetch를 지원하지만, 현재 사용 중인 Node 버전에서는 지원 X

⇒ **polyfill(폴리필)** 을 이용해 해결

[GitHub에서 만든 fetch polyfill](https://github.com/github/fetch)

#### 설치

```
npm i -D whatwg-fetch
```

#### setupTests.ts 파일에 import

```
import 'whatwg-fetch'
```

#### hooks/useFetchProducts.ts 원상태로 되돌리기

```ts
export default function useFetchProducts() {
    const url = 'http://localhost:3000/products';
    const {data} = useFetch<ProductsResult>(url);
    if (!data) {
        return [];
    }

    return data.products;
}
```

<br>

## 3. polyfill(폴리필)

[Polyfill](https://developer.mozilla.org/ko/docs/Glossary/Polyfill)
[모던 자바스크립트 - 폴리필](https://ko.javascript.info/polyfills)
[GitHub에서 만든 fetch polyfill](https://github.com/github/fetch)  

기본적으로 지원하지 않는 이전 브라우저에서 **최신 기능**을 제공하는 데 필요한 코드 (일반적으로 웹의 JavaScript)

자바스크립트 엔진을 만드는 각 조직은 나름대로 우선순위를 매겨 명세서 내 어떤 기능을 먼저 구현할지 결정  
명세서에 등록된 기능보다 초안(draft)에 있는 제안을 먼저 구현하기로 결정하는 경우도 존재  
구현 난도가 높아서 이런 결정을 내리는 경우도 있지만, 구미를 당기지 않아 이런 결정을 내리기도 함  
엔진이 **표준 전체를 지원하지 않고 일부만 지원**하는 건 흔한 일
