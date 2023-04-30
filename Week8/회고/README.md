# 한 주를 마치며

## 회고

이번 주는 비교적 쉽다고 생각 들어 그냥 지나칠 수 있는 CSS in JS에 대해 배웠다.
디자인 시스템이 무엇이고 어떤 요소가 있는지, 다양한 사례를 둘러보며 학습했다. 
리액트와 JSX를 처음 사용하면서 class를 **className**이라고 쓴다고 배웠는데 그 이유와, 브라우저에서는 어떻게 변경되어 나오는지 알게 되었다.

자바스크립트와 CSS를 같이 사용하면 CSS in JS라고 생각했는데, 
자바스크립트를 이용해 **스타일 컴포넌트** 형태로 작성하는 것이 핵심이었다. 
강의에서 소개된 라이브러리 중에 JSS를 제외하고 다 사용해 봤었는데, 
아무래도 Styled Components를 가장 많이 써서 그런지 친숙했다. 

Styled Components의 장점도 제대로 학습했다. **styled와 css**가 어떤 역할을 하는지도 처음 알았다.
상속받기, props, attrs 등 Styled Components를 제대로 활용하기 위한 방법도 배웠다.
돌이켜보면 Styled Components 공식 문서를 제대로 본 기억이 없었는데 이번 기회로 둘러보게 됐다.  

**GlobalStyle** 파일을 그냥 복사해서 사용하곤 했는데, 각각 어떤 내용이 담겨있는지 짚고 넘어가서 유익했다.
특히 font-size: 62.5% 라는 팁이 유용했다. 항상 상대 단위 때문에 복잡했었는데 너무 좋은 팁이다.
Context API를 기반으로 한 **ThemeProvider**를 이용해 다크 모드 구현하기를 해봤는데 쉽고 간단했다. 
낯설었던 Context API와도 가까워지고 있다. 

<br>

## 💡 새로 알게된 내용

- Design System
  - 디자인 시스템의 개념 
- CSS in JS
  - CSS in JS의 개념 
- styled-components
  - styled-components 장점
  - styled의 역할 
  - 기존 컴포넌트에 스타일 입히기
- props와 attrs
  - styled-components의 css
- Global Style & Theme
  - GlobalStyle
  - ThemeProvider


## 🔎 이해가 깊어진 내용

- Design System
  - 반응형 웹
- Style Basics
  - className
  - Inline Style
  - 의미있는 마크업
- styled-components
  - 상속 받기 
- props와 attrs
  - props 사용 방법 
- Global Style & Theme
  - word-break 속성 
  - .d.ts 파일


## 🤔 학습이 더 필요한 내용

- Style Basics
  - Semantic tag
- CSS in JS
  - 성능 이슈와 대안 
- props와 attrs
  - attrs 사용 방법 
- Global Style & Theme
  - Theme 사용 방법 
