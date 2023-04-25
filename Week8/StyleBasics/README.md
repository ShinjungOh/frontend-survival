# 2. Style Basics

## 1. Basic: Class

[스타일링과 CSS](https://ko.reactjs.org/docs/faq-styling.html)

`index.html` 파일에 간단히 CSS 추가하는 방법 

### id 선택자

`#id명` 하나만 존재

```tsx
<style>
    #greeting {
        color: #00F;
    }
</style>
```

### class 선택자
   
`.class명` 여러 요소에 중복 사용 가능

```tsx
<style>
    .greeting {
        color: #00F;
    }
</style>
```

### className

자바스크립트에서의 class 예약어와 겹치기 때문에 JSX에서는 “className”으로 변경되어 지정  

CSS의 class는 일반적인 class와 비슷  
어떤 타입, 속성, 카테고리에서 속하는 지를 표현

```tsx
export default function Greeting() {
    return (
        <p className="greeting">
            Hello, world!
        </p>
    );
}
```

* 브라우저에서는 class로 자동 변경되어 나옴  
`<p class="greeting">Hello, world!</p>`

### 단점

CSS는 컴포넌트를 전제로 하고 있지 않음  
그래서 공통된 부분이 많을 때 재사용하기는 쉽지만, 컴포넌트 중심으로 빠르게 작업하려고 하면 불편할 때가 많음  
재사용은 그냥 컴포넌트를 사용하면 되는데, 그렇게 하기 어려운 것

⇒ 절충안으로 **아주 작은 스타일 그룹**을 클래스로 지정하는 방법도 존재  
Ex. Bootstrap, Tailwind CSS 등의 접근법 - primary, default 등

#### [Bootstrap](https://getbootstrap.com/)

CSS, JS 프레임워크  
하나의 코드로 휴대폰, 태블릿, 데스크탑까지 다양한 기기에서 작동하게 만들 수 있으며, 
다양한 기능을 제공하여 사용자가 쉽게 웹사이트를 제작, 유지, 보수할 수 있도록 도움

#### [Tailwind CSS](https://tailwindcss.com/) 

오픈 소스 CSS 프레임워크  
부트스트랩과 같은 다른 CSS 프레임워크와 달리, 버튼이나 테이블과 같은 요소에 대해 미리 정의된 일련의 클래스를 제공하지 않음

<br>

## 2. Basic: Inline Style

“style” 속성 활용  
* CSS는 아니고, 평범한 **JavaScript 객체**를 활용한 것 → 변수, 함수 등을 재사용하기 쉬움 
* 텍스트가 아니라서 실수하거나 불편할 때가 있음 → TypeScript의 힘으로 자동 완성, 타입 검사 등의 도움을 받을 수 있음

```tsx
export default function Greeting() {
    const style = {
        color: '#00F',
    };
    
    return (
        <p style={style}>
            Hello, world!
        </p>
    );
}
```

* 브라우저에서는 인라인 스타일로 적용됨  
```<p style="color: rgb(255, 0, 0);">Hello, world!</p>```

### 바로 사용하는 방법 

```tsx
export default function Greeting() {
    return (
        <p 
            style={{
                color: '#00F',
            }}
        >
            Hello, world!
        </p>
    );
}
```

### 자바스크립트 장점 활용 

자바스크립트에서 사용하는 표현으로 유연하게 쓸 수 있음    
코드 레벨에서 적용 가능  
연산 작업이 들어가므로 속도는 느려짐 

```tsx
const darkMode = false;

function primaryColor() {
    return darkMode ? '#F00' : '#00F';
}

export default function Greeting() {
    return (
        <p style={{
            color: primaryColor(),
        }}>
            Hello!
        </p>
    );
}
```

<br>

## 3. 의미있는 마크업

> **🙋🏻‍♀️️ 주의 사항**  
> 
> 의미있는 마크업을 하기 위해서는 HTML Element 들이 어떤 의미를 가지고 있는지 정확하게 알고 있어야 합니다.
> 레퍼런스를 참고하여 HTML 마크업에 대해 정확하게 이해한 뒤 정리하고 넘어가시면 좋습니다.
> 
> [💡 참고 레퍼런스 - MDN](https://developer.mozilla.org/ko/docs/Web/HTML/Reference)


className을 사용해서 스타일을 줘도 되지만, 인라인과 다를 것이 없어지기 때문에, 의미있는 마크업을 사용하는 것이 좋음

```
<p className="greeting font-size-16">
```

### Semantic tag

[Semantic HTML](https://en.wikipedia.org/wiki/Semantic_HTML)  
[Semantics - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html)

> 📝 **Semantic** 의미의, 의미론적인

시맨틱 태그는 의미를 부여한 태그

### 장점

1. HTML 문서의 가독성과 유지보수가 쉬워짐
2. 웹 접근성이 증가 
3. 검색엔진이 검색을 수행할 때 HTML내의 태그를 분석 가능   
   * `<article>` 태그가 사용된 콘텐츠는 재배포할 수 있는 콘텐츠로 인식 
   * `<section>` 태그로 묶은 콘텐츠는 재배포를 금지하는 콘텐츠로 인식


> ✍️ **조각글**
>
> 과제를 할 때 `<table>` 관련 요소를 몰라서 헤맸었다. HTML 요소에 대한 체계적인 학습이 필요하다고 느꼈다.
> 코딩을 하면서 그때 그때 MDN 등 문서를 찾아보며 잘못 알고있는 내용이나 부족하게 알던 내용을 학습하고 있다.
> 시맨틱하게 작성하는 것이 중요하다고 알고 있는데, 각 태그의 세부 요소를 잘 숙지하고 사용하도록 하자.
