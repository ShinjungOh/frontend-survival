# 5. usehooks-ts

## 1. usehooks-ts

[usehooks-ts](https://usehooks-ts.com/)

모든 Hook에 대한 코드가 웹 사이트에 직접 노출됨

```
npm i usehooks-ts
```

### [useBoolean](https://usehooks-ts.com/react-hook/use-boolean)

참/거짓을 다룰 땐 toggle 같이 의도가 명확한 함수를 쓰는 게 좋다.

→ 위에서 만든 TimerControl에 써보자.

### [useEffectOnce](https://usehooks-ts.com/react-hook/use-effect-once)

의존성 배열을 빈 배열로 넣어서 한 번만 실행하는 Effect를 잡아줄 때가 많은데, 이걸 쓰면 더 명확히 드러난다.

→ 위에서 만든 useFetchProducts에 써보자.


- [use-http](https://use-http.com/)

조금 더 복잡해도 괜찮다면, 캐시 이슈를 고려한 좋은 대안이 있다.

- [SWR](https://swr.vercel.app/ko)
- [TanStack Query](https://tanstack.com/query)


### [useFetch](https://usehooks-ts.com/react-hook/use-fetch)

정말 간단히 쓸 때 좋음.

몇 가지 기능이 살짝 더 있는 useFetch 라이브러리가 따로 있다.

### [useInterval](https://usehooks-ts.com/react-hook/use-interval)

React에서 setInterval 등을 쓸 때는 주의해야 할 부분이 있어서 Custom Hook을 만들어서 해결해야 함.

- [useEffect 관점](https://overreacted.io/ko/a-complete-guide-to-useeffect/)
    - [React에서의 타이머 part 1 : setInterval 말고 이것! - 코드종님 영상](https://youtu.be/2tUdyY5uBSw)
- [Ref 활용](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)


### [useEventListener](https://usehooks-ts.com/react-hook/use-event-listener)

모든 종류의 이벤트를 확인할 수 있음. 특히 dispatchEvent로 전달되는 커스텀 이벤트에 반응하기 좋다. (강력 추천!)

### [useLocalStorage](https://usehooks-ts.com/react-hook/use-local-storage)

localStorage와 JSON으로 객체 영속화.

이벤트를 통해(dispatchEvent + useEventListener) 다른 컴포넌트와 동기화하는 게 매우 중요한 특징.


### [useDarkMode](https://usehooks-ts.com/react-hook/use-dark-mode)

<br>

## 2. swr

<br>

## 3. react-query
