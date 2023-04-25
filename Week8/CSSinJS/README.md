# 3. CSS in JS

## 1. CSS

Cascading Style Sheets   
HTML이나 XML(SVG, XHTML 포함)로 작성된 문서의 표시 방법을 기술하기 위한 스타일 시트 언어  
웹 페이지에 스타일과 레이아웃을 적용할 때 사용

<figure><img src="https://github.com/ShinjungOh/TIL/blob/main/Images/css-declaration.png?raw=true" alt="css-declaration" width="100%"></figure>

> **🙋🏻‍♀️️ 주의 사항**
>
> CSS 를 잘 사용하기 위해선 CSS 자체에 대해서 잘 알고 있어야 합니다.
> 레퍼런스를 참고하여 CSS 를 정리해봅시다.
> 스타일 자체는 굉장히 많기 때문에 자주 사용하는 것들 위주로 찾아서 정리하시면 좋습니다.
> 특히나 선택자, 박스모델, media query, Flexbox, Grid 정도는 필수적으로 알고 계셔야 합니다.
>
> 💡 참고 레퍼런스
> - [CSS - MDN](https://developer.mozilla.org/ko/docs/Web/CSS/Reference)
> - [flexbox - CSS-tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
> - [Grid - CSS-tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)

<br>

## 2. CSS in JS

[CSS in JS](https://en.wikipedia.org/wiki/CSS-in-JS)

자바스크립트를 이용해 **스타일 컴포넌트** 형태로 사용하는 것이 핵심

```tsx
import styled from 'styled-components';
// Create a component that renders a <p> element with blue text
const BlueText = styled.p`
 color: blue;
`;

<BlueText>My blue text</BlueText>
```

### 라이브러리

* [Emotion](https://emotion.sh/docs/introduction)
* [Styled Components](https://styled-components.com/)
* [JSS](https://cssinjs.org/?v=v10.10.0)
* [Tailwind CSS](https://tailwindcss.com/)

### 참고 레퍼런스 

#### [A Unified Styling Language (2017)](https://blog.rhostem.com/posts/2017-06-24-unified-styling-language)

1. Scoped styles
2. Critical CSS
3. Smarter optimisations
4. Package management
5. Non-browser styling

#### [All You Need To Know About CSS-in-JS (2017)](https://d0gf00t.tistory.com/22)

1. **Thinking in components** ✅ 컴포넌트로 사고할 수 있다는 것이 핵심 
2. CSS-in-JS **leverages the full power of the JavaScript ecosystem** to enhance CSS.
3. True rules isolation
4. Scoped selectors
5. Vendor Prefixing
6. Code sharing
7. **Only the styles which are currently** in use on your screen are also in the DOM (react-jss).
8. Dead code elimination
9. Unit tests for CSS

#### [Most popular CSS-in-JS libraries](https://npmtrends.com/aphrodite-vs-emotion-vs-glamorous-vs-jss-vs-radium-vs-styled-components-vs-styletron)

2023년 1월 기준으로 styled-components > JSS > Emotion 순서

### 성능 이슈

[CSS-in-JS와 성능 (2021)](https://hyeonseok.com/blog/877)    
[Why We're Breaking Up with CSS-in-JS (2022)](https://bit.ly/3g6QufF)  
→ CSS 파일과 JS 파일 로딩의 차이

CSS는 병렬로 로딩할 수 있고 속도가 빠른데,   
JS를 이용하면 로딩이 다 끝나고 나서야 무언가를 진행할 수 있음  


### 대안

- [Linaria](https://linaria.dev/)
    - Zero-Runtime CSS in JS
    - CSS를 평범한 텍스트로 작성
    - React에 종속적이지 않지만, React Styled Component도 지원함
- [vanilla-extract](https://vanilla-extract.style/)
    - Zero-runtime Stylesheets in **TypeScript**
    - CSS를 오브젝트 형태로 표현
    - React의 Inline Style과 유사
    - React와 무관하게 사용 가능
