# 4. usestore-ts

## 1. usesotre-ts

[usestore-ts](https://usestore-ts.com/)

### 사용 방법

```ts
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
import {container} from 'tsyringe';

import {useStore} from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
    const store = container.resolve(CounterStore);
    return useStore(store);
}
```

#### 비동기 함수에 @Action 추가  

⚠️ 비동기 함수에 @Action을 붙이면 다르게 작동할 수 있다는 점에 주의  
별도의 액션을 만들면 신경 쓸 부분이 줄어듦

```ts
@singleton()
@Store()
class PostStore {
    posts: Post[] = [];

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

<br>

## 2. useSyncExternalStore

[useSyncExternalStore](https://beta.reactjs.org/reference/react/useSyncExternalStore)  
[FECONF 2022 - 상태관리 이 전쟁을 끝내러 왔다](https://youtu.be/KEDUqA9JeIo)  


external store를 구독할 수 있는 React Hook  

```js
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```

