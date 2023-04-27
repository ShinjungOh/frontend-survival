# 5. props와 attrs

## 1. Props

[Passed props](https://styled-components.com/docs/basics#passed-props)

속성을 받아서 쓰고 싶을 때 유용

대상이 단순한 요소일 경우, styled-components는 HTML 속성을 통해 DOM으로 전달  
사용자 지정 React 컴포넌트인 경우, 스타일 컴포넌트는 모든 props를 통과함  
**템플릿 리터럴**을 이용해서 조건부 스타일 등을 표현    

* 활성화 여부 표현
* 특정 스타일을 잡아주고 싶을 때
* 조건부 스타일링 가능

### 사용 방법 

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/3ad63a123e02da769ce41ef2014eb7ab5b350457)

```tsx
import { useBoolean } from 'usehooks-ts';

import styled, { css } from 'styled-components';

type ParagraphProps = { 
    active?: boolean;
}

const Paragraph = styled.p<ParagraphProps>`
    color: ${(props) => (props.active ? '#F00' : '#888')};
    ${(props) => props.active && css`
    	font-weight: bold;
    `}
`;

export default function Greeting() {
    const { value: active, toggle } = useBoolean(false);
    
    return (
        <div>
            <Paragraph>Inactive</Paragraph>
            <Paragraph active>Active</Paragraph>
            <Paragraph active={active}>
                Hello, world
                {' '}
                <button type="button" onClick={toggle}>
                    Toggle
                </button>
            </Paragraph>
        </div>
    );
}
```

💡 styled-components 의 { **css** } 를 활용하면, 단순 문자열이 아닌 CSS 처럼 사용할 수 있음  
💡 `<ParagraphProps>` 타입을 사용하지 않으려면 `<{ active: boolean }>` 의 형태로 직접 적는 것도 가능 

> ✍️ **조각글**
> 
> 왜 props를 이용할 때 `${(props) => props.active && css}` 의 방식으로 적는지 몰랐는데 
> `css`를 제외하고 작성해보니 불편함을 알게 되었다. 스타일 코드를 문자열로 인식해서 CSS를 작성하기 어려웠다.
> 잘 모르면서 원래 그런가보다 하고 썼던 `styled`를 사용하는 이유에 대해서도 알게 됐고, `css`의 존재 이유도 알게 되어 즐겁다.

<br>

## 2. attrs  

[Attaching additional props](https://styled-components.com/docs/basics#attaching-additional-props)  
[.attrs](https://styled-components.com/docs/api#attrs)

**기본 속성을 추가**할 수 있음  
불필요하게 반복되는 속성을 처리할 때 유용

* `<button>` 등을 만들 때 적극 활용
    * `type="button"` 등을 무조건 써야 하는데, 안 쓰면 오류가 발생
    * `styled.button.attrs({ type: 'button' })` 형태로 작성
    
### .attrs

스타일 컴포넌트에 일부 props를 연결하는 연결 가능한 메소드  
첫 번째이자 유일한 인수는 컴포넌트의 나머지 props에 병합될 객체

각 래퍼가 `.attrs`의 **중첩 사용을 재정의(override)** 할 수 있음  
스타일시트에서 나중에 정의된 css 속성이 이전 선언을 덮어쓰는 방식과 유사

### 사용 방법

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/825627f6c84c10c65761bdef5a715d8b96ec3cd8)

```tsx
import styled from 'styled-components';

const Button = styled.button.attrs({
    type: 'button',
})`
    border: 1px solid #888;
    background: transparent;
    cursor: pointer;
`;

export default Button;
```

```tsx
// button의 타입을 지정할 수 있음 
type ButtonProps = { 
    type?: 'button' | 'submit' | 'reset'; 
    active?: boolean;
};

// attrs의 <ButtonProps>
const Button = styled.button.attrs<ButtonProps>(props => ({
  type: props.type ?? 'button',
}), 
// style을 위한 <ButtonProps> 
)<ButtonProps>` 
    background: #FFF;
    color: #000;
    border: 1px solid ${(props: ButtonProps) => (props.active ? '#F00' : '#888')};
	
	${props => props.active && css`
	    background: #00F;
	    color: #FFF;
	`}
`;

export default function Switch() {
  const {value: active, toggle} = useBoolean(false);

  return (
      // button 타입을 지정하지 않은 경우, 스타일 컴포넌트에서 attrs 설정대로 지정됨
      <Button onClick={toggle} active={active}>
        On/Off
      </Button>
  );
}
```
