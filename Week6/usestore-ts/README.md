# 4. usestore-ts

## 1. Action을 메소드로 처리하기

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/1f4bb95e565dcf29daf5e90358f799132a21e8d0)

> 💡 Redux와 달리 단일 스토어가 아니기에, 스토어 여러 개를 만들어서 사용하면 됨

### CounterStore, useCounterStore 만들기

```ts
// src/stores/ObjectStore.ts

type Listener = () => void;

export default class ObjectStore {
    private listeners = new Set<Listener>();

    addListener(listener: Listener) {
        this.listeners.add(listener);
    }

    removeListener(listener: Listener) {
        this.listeners.delete(listener);
    }

    protected publish() {
        this.listeners.forEach((listener) => listener());
    }
}
```

```ts
// src/stores/CounterStore.ts

import {singleton} from 'tsyringe';

import ObjectStore from './ObjectStore';

@singleton()
export default class CounterStore extends ObjectStore {
    count = 0;

    /**
     * 카운트를 증가시킨다.
     */
    increase(step = 1) {
        this.count += step;
        this.publish();
    }

    /**
     * 카운트를 감소시킨다.
     */
    decrease() {
        this.count -= 1;
        this.publish();
    }
}
```

```ts
// src/hooks/useObjectStore.ts

import {useEffect} from 'react';

import useForceUpdate from './useForceUpdate';

import ObjectStore from '../stores/ObjectStore';

export default function useObjectStore<T extends ObjectStore>(store: T): T {
    const forceUpdate = useForceUpdate();

    useEffect(() => {
        store.addListener(forceUpdate);

        return () => store.removeListener(forceUpdate);
    }, [store, forceUpdate]);

    return store;
}
```

```ts
// src/hooks/useCounterStore.ts 

import {container} from 'tsyringe';

import useObjectStore from './useObjectStore';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
    const store = container.resolve(CounterStore);

    return useObjectStore(store);
}
```

<br>

## 2. usesotre-ts

[usestore-ts](https://usestore-ts.com/)

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/61b0546c148f9c6e49df2e3548901f6c8bc96c83)

<em>React state management library</em>

> 💡 내부적으로 어떻게 동작하는지 이해하고 사용할 것  
> 타입스크립트를 잘 모른다면 어려울 수 있음   
> 남에게 설명할 때 내부적으로는 이런 동작을 하고 있으며 어떤 방식으로 작동하는지 얘기할 수 있어야 함  

### 사용 방법

#### 패키지 설치

```
npm install usestore-ts
```

#### `tsconfig.json` 파일 decorators 사용 옵션 변경

```
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
```

#### Store 생성

```ts
// CounterStore.ts

import {singleton} from 'tsyringe';

import {Store, Action} from 'usestore-ts';

@singleton()
@Store()
export default class CounterStore {
    count = 0;

    @Action()
    increase(step = 1) {
        this.count += step;
    }

    @Action()
    decrease() {
        this.count -= 1;
    }
}
```

#### Custom Hook 작성

```ts
// useCounterStore.ts

import {container} from 'tsyringe';

import {useStore} from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
    const store = container.resolve(CounterStore);

    return useStore(store);
}
```

#### 불러와서 사용하기

```tsx
// Counter.tsx

export default function Counter() {
    const [{count}] = useCounterStore();
    // 내용
}
```

```tsx
// CounterControl.tsx

export default function CounterControl() {
	const [, store] = useCounterStore();
    // 내용
}
```

#### 비동기 함수에 @Action 추가

⚠️ 비동기 함수에 @Action을 붙이면 다르게 작동할 수 있다는 점에 주의  
별도의 액션을 만들면 신경 쓸 부분이 줄어듦

* start, complete 붙은 것이 액션 

```ts
@singleton()
@Store()
class PostStore {
    posts: Post[] = [];

    // 여기에 @Action() 를 붙일 경우 프로미스를 리턴해서 publish
    async fetchPosts() {
        this.startLoading();

        const posts = await fetchPosts();

        this.completeLoading(posts);
    }

    @Action()
    startLoading() {
        this.posts = [];
    }

    @Action()
    completeLoading(posts: Post[]) {
        this.posts = posts;
    }
}
```

### Immer

[Immer](https://immerjs.github.io/immer/)

불변성을 유지해주는 코드를 편리한 방식으로 작업할 수 있게 해주는 라이브러리  
오브젝트 전체를 복사해서 주고, 그것을 이용할 수 있도록 되어 있음  
[Redux Toolkit](https://redux-toolkit.js.org/) 에도 기본으로 적용

```js
import produce from "immer"

const nextState = produce(baseState, draft => {
    draft[1].done = true
    draft.push({title: "Tweet about it"})
})
```

<br>

## 3. useSyncExternalStore

[useSyncExternalStore](https://beta.reactjs.org/reference/react/useSyncExternalStore)  
[FECONF 2022 - 상태관리 이 전쟁을 끝내러 왔다](https://youtu.be/KEDUqA9JeIo)

external store를 구독할 수 있는 React Hook

```js
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot ?)
```
