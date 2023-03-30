# 5. usehooks-ts

## 1. usehooks-ts

[usehooks-ts](https://usehooks-ts.com/)

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/0d07df76f62ce7087374059881142992d5099d07)

모든 Hook에 대한 코드가 웹 사이트에 직접 노출됨

```
npm i usehooks-ts
```

### [useBoolean](https://usehooks-ts.com/react-hook/use-boolean)

참/거짓을 다룰 땐 toggle 같이 의도가 명확한 함수를 사용하는 것을 권장

#### 예시

```jsx
// TimerControl.tsx

const {value: playing, toggle: togglePlaying} = useBoolean();

const handleClick = () => {
    togglePlaying();
};

return (
    <button type='button' onClick={handleClick}>Toggle</button>
)

// 또는 handleClick 대신에 togglePlaying 직접 사용 

return (
    <button type='button' onClick={togglePlaying}>Toggle</button>
)
```

### [useEffectOnce](https://usehooks-ts.com/react-hook/use-effect-once)

의존성 배열을 빈 배열로 넣어서 한 번만 실행하는 Effect를 잡아줄 때가 많은데, useEffectOnce를 사용하면 한 번만 실행하는 것이 명확해짐  
표현력의 증대 

#### 예시

```jsx
// useFetchProducts.ts

useEffectOnce(() => {
    const fetchProducts = async () => {
        const url = 'http://localhost:3000/products';
        const response = await fetch(url);
        const data = await response.json();
        setProducts(data.products);
    };

    fetchProducts();
});
```

### [useFetch](https://usehooks-ts.com/react-hook/use-fetch)

정말 간단히 쓸 때 좋음

#### 예시

```jsx
export default function useFetchProducts() {
    const url = 'http://localhost:3000/products';
    const {data} = useFetch(url);
    if (!data) {
        return [];
    }
    return data.products;
}
```

* 데이터가 로딩 중일 때는 문제가 될 수 있으므로 예외 처리

#### 기타 라이브러리 

몇 가지 기능이 살짝 더 있는 useFetch 라이브러리

- [use-http](https://use-http.com/)

조금 더 복잡해도 괜찮다면, 캐시 이슈를 고려한 좋은 대안

- [SWR](https://swr.vercel.app/ko)
- [TanStack Query](https://tanstack.com/query) (React Query)

### [useInterval](https://usehooks-ts.com/react-hook/use-interval)

React에서 setInterval 등을 쓸 때는 주의해야 할 부분이 있어서 Custom Hook을 만들어서 해결해야 함

- [useEffect 관점](https://overreacted.io/ko/a-complete-guide-to-useeffect/#%EC%9D%98%EC%A1%B4%EC%84%B1%EC%9C%BC%EB%A1%9C-%EA%B1%B0%EC%A7%93%EB%A7%90%EC%9D%84-%ED%95%98%EB%A9%B4-%EC%83%9D%EA%B8%B0%EB%8A%94-%EC%9D%BC)
    - [React에서의 타이머 part 1 : setInterval 말고 이것! - 코드종님 영상](https://youtu.be/2tUdyY5uBSw)
- [Ref 활용](https://overreacted.io/making-setinterval-declarative-with-react-hooks/#refs-to-the-rescue)

### [useEventListener](https://usehooks-ts.com/react-hook/use-event-listener)

모든 종류의 이벤트를 확인 가능  
💡 특히 dispatchEvent로 전달되는 커스텀 이벤트에 반응하기 좋음 

#### useClickAnyWhere()에서의 예시

```jsx
import { useEventListener } from 'usehooks-ts'

type Handler = (event: MouseEvent) => void

function useClickAnyWhere(handler: Handler) {
  useEventListener('click', event => {
    handler(event)
  })
}

export default useClickAnyWhere
```

### [useLocalStorage](https://usehooks-ts.com/react-hook/use-local-storage)

localStorage와 JSON으로 객체 영속화  
이벤트를 통해(dispatchEvent + useEventListener) 다른 컴포넌트와 동기화하는 게 매우 중요한 특징.

### [useDarkMode](https://usehooks-ts.com/react-hook/use-dark-mode)

OS에서 설정된 다크모드 여부(초깃값)를 알 수 있음

```jsx
const isDarkOS = useMediaQuery(COLOR_SCHEME_QUERY)
```

<br>

## 2. SWR

[SWR](https://swr.vercel.app/ko)

SWR 명칭은 HTTP RFC 5861(opens in a new tab)에 의해 알려진 HTTP 캐시 무효 전략인 **stale-while-revalidate**에서 유래    
SWR은 먼저 캐시(스태일)로부터 데이터를 반환한 후, fetch 요청(재검증)을 하고, 최종적으로 최신화된 데이터를 가져오는 전략

SWR을 사용하면 컴포넌트는 지속적이며 자동으로 데이터 업데이트 스트림을 받게 됨  
UI는 항상 빠르고 반응적

### 장점

단 한 줄의 코드로 프로젝트 내의 데이터 가져오기 로직을 단순화할 수 있으며, 다음의 기능들을 바로 사용할 수 있음

* **빠르고, 가볍고, 재사용 가능**한 데이터 가져오기
* 내장된 **캐시** 및 요청 중복 제거
* **실시간** 경험
* 전송 및 프로토콜에 구애받지 않음
* SSR / ISR / SSG support
* TypeScript 준비
* React Native

<br>

## 3. React Query

[TanStack Query](https://tanstack.com/query)

TS/JS, React, Solid, Vue 및 Svelte를 위한 강력한 비동기 상태 관리  

> <em>세분화된 상태 관리, 수동 다시 가져오기 및 끝없는 비동기식 스파게티 코드 그릇을 버리십시오. 
> **TanStack Query는 개발자와 사용자 경험 모두를 직접적으로 개선하는** 
> 선언적이고 항상 최신 상태의 자동 관리 쿼리 및 변형을 제공합니다.</em>

### 장점 

#### 선언적 및 자동

React Query에 데이터를 가져올 위치와 필요한 최신 정보를 알려주면 나머지는 자동으로 처리   
zero-configuration으로 즉시 캐싱, 백그라운드 업데이트 및 stale 데이터를 처리

#### 단순하고 친숙함

promise, async/await 작업 방법을 알고 있다면 React Query를 사용할 수 있음   
관리해야 할 전역 상태, 리듀서, 정규화 시스템 또는 이해할 무거운 구성이 없음  
데이터를 해결하는(또는 오류를 발생시키는) 함수를 전달하기만 하면 됨 

#### 확장 가능

React Query는 모든 사용 사례에 맞는 노브와 옵션을 사용하여 쿼리의 각 옵저버 인스턴스까지 구성할 수 있음  
전용 devtools, 무한 로딩 API 및 데이터를 쉽게 업데이트할 수 있는 일급 mutation tool 제공
