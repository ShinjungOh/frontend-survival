# 2. React Testing Library

## 1. React Testing Library

[React Testing Library 깃허브](https://github.com/testing-library/react-testing-library)  
[React Testing Library 공식 문서](https://testing-library.com/docs/react-testing-library/intro/)  
[jest-dom](https://github.com/testing-library/jest-dom)

💡 React 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구  
UI 테스트에 특화된 라이브러리  
E2E Test처럼 사용 가능 → 브라우저에서 사용자가 실제로 사용하듯이 테스트

> 🛠 **기존의 도구**
>
> [enzyme](https://enzymejs.github.io/enzyme/)  
> 사용자 입장에서 보다는 해킹하는 느낌으로 사용하도록 되어 있었음  
> React Testing Library도 구현 자체는 해킹하는 것처럼 되어 있지만, 사용자 입장에 가깝게 만들어짐
>
> 🤓 **React Testing Library는?**  
> 웹 브라우저가 아닌 곳에서는 document를 쓸 수 없지만, 그런 것을 가능하게 해줌   
> jest는 node에서 돌아가는데 React Testing Library는 DOM 관련 메소드를 사용할 수 있고, 그 위에 react를 얹어서 돌리면 테스트 가능
>
> ```js
> const div = document.createElement('div')
> ```

### 장점

* 빠르게 작동
* 테스트 시나리오 작성이 쉬움

`render`를 이용하면 Main.tsx의 render와 비슷하지만 훨씬 쉽게 사용 가능

```js
import {render} from '@testing-library/react';
```

```js
// Main.tsx

root.render((
    <React.StrictMode>
        <App/>
    </React.StrictMode>
));
```

<br>

## 2. given - when - then 패턴

모든 BDD 시나리오에 있는 3가지 핵심 요소

* GIVEN (context, 문맥 설명)
* WHEN (action, 동작 설명) : ~ 한 동작을 하면
* THEN (outcome, 결과 설명) : ~ 이렇게 된다

```js
context('with only one arguments', () => {
    it('returns the same numbers', () => {
        // When
        const result = add(2);

        // Then
        expect(add(2)).toBe(2);
    });
});
```

케이스를 나눠 코드를 짜기 때문에 표현력이 좋아지고, 다양한 상황에 대해 고민할 수 있음

## 3. 테스트 코드 작성하기

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/597e5a25f821bd61e8b5cce82ef0c7435a272c8a)

### 컴포넌트의 인터페이스 점검

테스트 코드(컴포넌트를 사용하는 코드)를 작성하면서 해당 컴포넌트의 인터페이스를 점검 가능

```js
// TextField.test.tsx

import {render, screen} from '@testing-library/react';

import TextField from './TextField';

test('TextField', () => {
    // Given
    const text = 'Tester';
    const setText = () => {
        // do nothing...
    };

    // When
    render((
        <TextField
            label="Name"
            placeholder="Input your name"
            text={text}
            setText={setText}
        />
    ));

    // Then
    screen.getByLabelText('Name');
    screen.getByPlaceholderText(/name/);
    screen.getByDisplayValue(text);
});
```

기존의 문제

* label이 빠져있는 문제
* text 같이 범용적인 표현을 사용하지 않은 문제

> **테스트부터 작성**했거나 **빠르게 테스트 코드를 작성**했다면, 작성하기 전 또는 직후에 문제를 발견해서 수정할 수 있었을 것     
> ⚠️ 시간이 지나면 해당 코드에 대한 지식이 감소하고, 자신감도 감소하기 때문에 건드리기 힘든 코드가 되니 주의

### BDD 스타일로 코드 수정, 입력 기능 테스트

BDD 스타일로 코드를 바꾸고, 입력 등이 잘 작동하는지 확인

#### 1. context 분리

```js
import {render, screen, fireEvent} from '@testing-library/react';

import TextField from './TextField';

const context = describe;

describe('TextField', () => {
    // Given
    const label = 'Name';
    const text = 'Tester';

    const setText = jest.fn(); // 모킹

    it('renders elements', () => {
        // When
        render((
            <TextField
                label={label}
                placeholder='Input your name'
                text={text}
                setText={setText}
            />
        ));

        // Then
        screen.getByLabelText(label);
        screen.getByPlaceholderText(/name/);
        screen.getByDisplayValue(text);
    });

    context('when user enters name', () => {
        it('calls "setText" handler', () => {
            // Given
            render((
                <TextField
                    label={label}
                    placeholder='Input your name'
                    text={text}
                    setText={setText}
                />
            ));

            // When
            fireEvent.change(screen.getByLabelText(label), {
                target: {value: 'New Name'},
            });

            // Then
            expect(setText).toBeCalledWith('New Name');
        });
    });
});
```

#### 2. render 함수 생성, 모킹 함수 초기화 처리

describe-context로 나눠줄수록 수월하게 진행됨

```js
import {render, screen, fireEvent} from '@testing-library/react';

import TextField from './TextField';

const context = describe;

describe('TextField', () => {
    const text = 'Tester';
    const setText = jest.fn(); // 매 테스트마다 초기화 해주어야 함 

    beforeEach(() => {
        setText.mockClear(); // → 해당 대상만 clear
        // 또는 jest.clearAllMocks(); → 전부 다 clear
    });

    function renderTextField() {
        render((
            <TextField
                label="Name"
                placeholder="Input your name"
                text={text}
                setText={setText}
            />
        ));
    }

    function inputText(value: string) {
        fireEvent.change(screen.getByLabelText(label), {
            target: {value},
        });
    }

    it('renders an input control', () => {
        // When
        renderTextField();

        // Then
        screen.getByLabelText('Name');
    });


    context('when user enters name', () => {
        beforeEach(() => {
            // Given
            renderTextField();
        });

        it('calls "setText" handler', () => {
            // When
            inputText('New Name');

            // Then
            expect(setText).toBeCalledWith('New Name');
        });
    });
});
```

* 반복되는 코드를 Extract Function
* fireEvent 등을 통해 인터랙션만 검증
* 만약 복잡한 로직이 컴포넌트로부터 분리된다면, 여기서는 이것만 검증하면 됨
    * Ex. setText에 숫자만 입력받게 하고 싶다면, 컴포넌트에서 그런 로직을 구현하는 것이 아니라 테스트 할 때 그렇게 동작하도록 작성하면 됨
    * 💡 컴포넌트의 책임이 아니게 됨

### API 요청 코드 모킹

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/99e9800609fc924ac9a7e88d07ec6b6f878356c2)

외부 의존성이 큰 코드(API 요청 등)를 작성할 경우, 해당 부분만 가짜로 구현

```js
// App.test.tsx

import {render, screen} from '@testing-library/react';

import App from './App';

jest.mock('./hooks/useFetchProducts', () => () => [
    {
        category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
    },
]);

test('App', () => {
    render(<App/>);

    screen.getByText('Apple');
});
```

매번 서버를 띄우기 어렵고, 실서버를 사용하기 어려운 문제를 방지하기 위해 테스트에서만 **가짜로 서버를 구현**  
프론트엔드는 일반적으로 백엔드와 소통하는 비중이 큼   
💡 이 부분을 하나씩 가짜 구현으로 바꾸다 보면 어려운 경우가 발생 → MSW 등 다른 대안을 고려

<br>

## 4. Mocking

[모의 객체](https://ko.wikipedia.org/wiki/%EB%AA%A8%EC%9D%98_%EA%B0%9D%EC%B2%B4)

가짜로 적는 것  
`테스트를 수행할 모듈`과 연결되는 `외부의 다른 서비스나 모듈`을   
`실제 사용하는 모듈`을 사용하지 않고 실제의 모듈을 **흉내내는 가짜 모듈**을 작성 → 테스트의 효용성을 높임    
자동화된 테스트를 수행하기 어려울 때 주로 사용

> 🛠 **TDD와의 관계**
>
> 테스트 주도 개발(TDD)에서는 **자동화된 테스트**가 필수적인 요소 중의 하나  
> 모의 객체를 이용하면 상당 부분의 테스트를 사용자의 개입 없이 자동화 할 수 있음

### 사용 예시 

1. 사용자 인터페이스(UI) 테스트 : 사용자의 반응이 필요한 테스트를 수행할 경우, 사용자가 테스트에 참여해야 하기 때문에 자동화된 테스트 수행이 어려움. 
   모의 객체를 이용해 사용자의 응답을 흉내내어 사용자의 개입 없이도 테스트를 수행
2. 데이터베이스(DB) 테스트 : 자료의 변경을 수반하는 데이터베이스에 대한 작업을 테스트 하는 경우, 
   테스트 수행 후 매번 데이터베이스의 자료를 원래대로 돌려놔야 하는데, 모의 객체를 이용해 데이터베이스의 응답을 흉내내어 데이터의 변경 없이 테스트가 가능

<br>

## 5. Test fixture

[Test fixture](https://en.wikipedia.org/wiki/Test_fixture)

소프트웨어를 **일관되게 테스트**하기 위해 사용되는 환경  
한 곳에 몰아서 다른 곳에서 사용하기 편리함  

### 장점

* 각 테스트가 항상 동일한 설정으로 시작하기 때문에 테스트를 반복할 수 있음 
* 메소드를 다른 함수로 분리하고 각 기능을 다른 테스트에 재사용할 수 있음
* 테스트 코드 설계가 용이함 
* 이전 테스트 실행에서 남은 항목으로 작업하는 대신, 알려진 초기 상태로 테스트를 미리 구성

### 폴더 구조

#### 1. 직접 사용하는 경우

```
├── src
│  ├── App.test.tsx
│  ├── App.tsx
```

```
├── fixtures
│   ├── index.ts
│   └── products.ts
```

```js
// App.test.ts

import {render, screen} from '@testing-library/react';
import App from './App';
import fixtures from '../fixtures';

jest.mock('./hooks/useFetchProducts', () => () => fixtures.products);

test('App', () => {
    render(<App/>);

    screen.getByText('Apple');
});
```

```js
// fixtures/products.ts

const products = [
    {
        category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
    },
];

export default products;
```

```js
// fixtures/index.ts

import products from './products';

export default {
    products,
};
```

#### 2. __mocks__ 폴더를 분리할 경우

복잡해지면 이 방법을 사용 

```
│   ├── hooks
│   │   ├── __mocks__
│   │   │   └── useFetchProducts.ts
│   │   └── useFetchProducts.ts
```

```js
// App.test.ts

import {render, screen} from '@testing-library/react';
import App from './App';

// Jest.mock('./hooks/useFetchProducts', () => () => fixtures.products);
jest.mock('./hooks/useFetchProducts');

test('App', () => {
    render(<App/>);

    screen.getByText('Apple');
});
```

```js
// fixtures/products.ts

import fixtures from '../../../fixtures';

// Const useFetchProducts = () => fixtures.products; // 이렇게 써도 되지만 
const useFetchProducts = jest.fn(() => fixtures.products); // 모킹을 드러내기 위해 권장되는 방법  

export default useFetchProducts;
```
