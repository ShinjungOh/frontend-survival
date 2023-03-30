# 4. useRef & Custom Hook

## 1. useRef

[공식 문서의 useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)  
[beta 문서의 useRef](https://react.dev/reference/react/useRef)

컴포넌트의 생애주기 전체에 걸쳐서 유지되는 객체(컴포넌트가 없어질 때까지 동일한 객체가 유지)  

객체 자체가 값은 아니고, 값을 참조(reference)하기 위한 객체  
언제든지 값을 변경할 수 있음  

state와 상관 없이 어떤 값을 유지

> 🕰 **컴포넌트의 생애주기**
> 
> 컴포넌트가 생성되고, 없어질 때(VDOM으로 들어간 요소가 사라질 때) 까지

### 사용 방법

```jsx
const refContainer = useRef(initialValue);
```

useRef는 `.current` 프로퍼티로 전달된 인자(initialValue)로 초기화된 **변경 가능한 ref 객체를 반환**  
반환된 객체는 컴포넌트의 전 생애주기를 통해 유지

📦 본질적으로 useRef는 `.current` 프로퍼티에 변경 가능한 값을 담고 있는 상자

### 주의점 

useRef는 내용이 변경될 때 그것을 알려주지는 않음  
💡 상태(state)가 변경되면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하지만, 레퍼런스 객체의 현재 값(current)이 바뀌더라도 **렌더링에 영향을 주지 않음**

* .current 프로퍼티를 변형하는 것이 리렌더링을 발생시키지는 않음 
* React가 DOM 노드에 ref를 attach하거나 detach할 때 어떤 코드를 실행하고 싶다면 대신 콜백 ref를 사용할 것

### ref와 useRef

> 🌳 **ref**
>
> [Ref와 DOM](https://ko.reactjs.org/docs/refs-and-the-dom.html)
> 
> DOM에 접근하는 방법  
> `<div ref={myRef} />`를 사용하여 React로 ref 객체를 전달하면, React는 노드가 변경될 때마다 변경된 DOM 노드에 그것의 .current 프로퍼티를 설정


> 🎁 **ref와 useRef**     
> 
> ref 속성보다 useRef()가 더 유용       
> useRef Hook은 DOM ref만을 위한 것이 아님  
> 이 기능은 클래스에서 인스턴스 필드를 사용하는 방법과 유사한 어떤 가변값을 유지하는 데에 편리  
> `ref` 객체는 현재 프로퍼티가 변경할 수 있고 어떤 값이든 보유할 수 있는 일반 컨테이너  
> 이는 class의 인스턴스 프로퍼티와 유사   


⇒ useRef()가 순수 자바스크립트 객체를 생성하기 때문    
useRef()와 {current: ...} 객체 자체를 생성하는 것의 유일한 차이점은, useRef는 매번 렌더링을 할 때 동일한 ref 객체를 제공

### 용도

1. 컴포넌트가 사라질 때까지 동일한 값을 써야 하는 경우 ⇒ input 등의 ID 관리
2. (특히 useEffect 등과 함께 쓰면서 만나게 되는) 비동기 상황에서 현재 값을 제대로 쓰고 싶은 경우 
   * ❗️ Closure → 변수를 capture, bind를 깜빡하는 문제가 종종 발생

#### 예시 

절대로 쓸 일이 없는 억지로 꾸며낸 상황

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/4e69dd7b5afa28fa78f3494ad00b632209cb6493)

```jsx
// 5초 후에 현재의 입력값을 보려고 할 때 

useEffect(() => {
    setTimeout(() => {
        console.log(filterText);
    }, 5_000);
}, []);
```

* 현재값이 나오지 않음 → 시작할 때의 값이 들어갔기 때문

```jsx
  const query = useRef('');

  useEffect(() => {
    query.current = filterText;
  }, [filterText]);

  useEffect(() => {
    setTimeout(() => {
      console.log(query.current);
    }, 5_000);
  }, []);
```

* 현재값이 제대로 출력됨

### 클로저(closure)

[변수의 유효범위와 클로저](https://ko.javascript.info/closure)

클로저는 **외부 변수**를 기억하고 이 외부 변수에 접근할 수 있는 함수   
몇몇 언어에선 클로저를 구현하는 게 불가능하거나 특수한 방식으로 함수를 작성해야 클로저를 만들 수 있지만, **자바스크립트에선 모든 함수가 클로저**(예외 존재)

자바스크립트의 함수는 숨김 프로퍼티인 [[Environment]]를 이용해 자신이 어디서 만들어졌는지를 기억  
함수 본문에선 [[Environment]]를 사용해 외부 변수에 접근

#### 가비지 컬렉션

함수 호출이 끝나면 함수에 대응하는 렉시컬 환경이 메모리에서 제거 → 함수와 관련된 변수들은 이때 모두 사라짐(관련 변수를 참조할 수 없는 이유)  
자바스크립트에서 모든 객체는 도달 가능한 상태일 때만 메모리에 유지됨

호출이 끝난 후에도 여전히 도달 가능한 중첩 함수가 존재할 수 있음     
이때는 이 중첩함수의 [[Environment]] 프로퍼티에 외부 함수 렉시컬 환경에 대한 정보가 저장(도달 가능한 상태)  
함수 호출은 끝났지만 렉시컬 환경이 메모리에 유지되는 이유

#### 예외

[new Function](https://ko.javascript.info/new-function) 을 이용해 함수를 만들면 함수의 [[Environment]] 프로퍼티가 현재 렉시컬 환경이 아닌 **전역 렉시컬 환경**을 참조  
⇒ new Function을 이용해 만든 함수는 **외부 변수에 접근할 수 없고**, 오직 **전역 변수**에만 접근 가능

<br>

## 2. Custom Hook

[Custom Hook](https://ko.reactjs.org/docs/hooks-custom.html)

로직을 재사용하기 위한 제일 쉬운 방법  
Extract Function을 수행하면 됨(컴포넌트 로직을 함수로 뽑아내어 재사용)   
기본적으로 Hook의 디자인을 따르는, 기존과 같은 코드 

### 특징

#### 1. 이름이 use로 시작하는 자바스크립트 함수

* 컴포넌트 이름 : 대문자로 시작하는 PascalCase 
* Hook의 이름 : `use`로 시작하는 camelCase

React 컴포넌트와는 다르게 Custom Hook은 특정한 시그니처가 필요하지 않음    
일반 함수와 마찬가지로 무엇을 인수로 받아야 하며, 무엇을 반환해야 하는 지를 사용자가 결정할 수 있음

#### 2. Custom Hook은 다른 Hook을 호출 가능 

Hook은 함수이기 때문에 Hook 사이에서도 정보를 전달 가능

#### 3. Custom Hook은 독립된 state를 얻음

Custom Hook은 상태 관련 로직(구독을 설정하고 현재 변숫값을 기억하는 것)을 재사용하는 메커니즘  
각각의 Custom Hook에 대한 호출은 서로 독립된 state를 받음   
그 안의 state와 effect는 완전히 독립적  
하나의 컴포넌트 안에서 useState와 useEffect를 여러 번 부를 수 있고 이들은 모두 완전히 독립적

### Custom Hook 예제

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/a5bb3ccc042f37ecee52da7d937d7ccd0f85efed)

```jsx
// useFetchProducts.ts

export default function useFetchProducts() {
    const [products, setProducts] = useState<Product[]> ([]);

    useEffect(() => {
        const fetchProducts = async () => {
            const url = 'http://localhost:3000/products';
            const response = await fetch(url);
            const data = await response.json();
            setProducts(data.products);
        };

        fetchProducts();
    }, []);

    return products;
}
```

```jsx
// App.tsx

export default function App() {
    const products = useFetchProducts();
    
   return (
       <FilterableProductTable products={products} />
   );
}
```

### 장점 

* 컴포넌트 코드가 명확해짐
* 재사용 편리
* 일종의 캡슐화 ⇒ setProducts가 실수로 잘못 쓰일 문제를 해소
* Hook을 사용하여 트리에 컴포넌트를 더하지 않음 
* state를 이해하고 고민하지 않아도 쓸 수 있음(그냥 꺼내다 쓰면 됨)

<br>

## 3. Hook의 규칙

[Hook의 규칙](https://ko.reactjs.org/docs/hooks-rules.html)

Hook 호출은 규칙이 있어서 단순하게 쓰도록 노력해야 함  

1. Function Component 바로 안쪽(함수의 최상위)에서만 호출
2. Function Component 또는 Custom Hook에서만 호출

⚠️ 처음에는 콜백 함수나 조건문 안에서 Hook을 호출하는 실수를 저지르기 쉬우니 주의   
가능하면 바깥으로 빼기

```jsx
// TimerControl.tsx

if (playing) {
    const products = useFetchProducts();
    console.log(products);
}
```

* 오류 발생

```jsx
// custom hook을 불러오는 곳

const { products, fetchProducts } = useFetchProducts();
```

* 애초에 custom hook을 만들 때 신경써야 함  
