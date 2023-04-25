# 1. Design System

## 1. 반응형 웹 디자인(Responsive web design)

[반응형 웹 디자인](https://ko.wikipedia.org/wiki/%EB%B0%98%EC%9D%91%ED%98%95_%EC%9B%B9_%EB%94%94%EC%9E%90%EC%9D%B8)

하나의 웹사이트에서 PC, 스마트폰, 태블릿 PC 등 접속하는 디스플레이의 종류에 따라 **화면의 크기가 자동으로 변하도록** 만든 웹페이지 접근 기법  
반대말은 디바이스별로 별도의 웹사이트를 제작하는 **적응형 웹(adaptive web)**

### 장점 

1. **하나의 공용 웹사이트**  
PC용, 모바일용으로 각각 제작하지 않고, 하나의 공용 웹사이트로 다양한 디바이스에 대응 가능  

2. **동일 URL**  
PC용 URL과 모바일용 URL이 동일하기 때문에, 검색 포털 등 광고를 통한 사용자 
접속을 효율적으로 관리할 수 있음

3. **효율적인 업데이트**   
웹 페이지 내용을 수정할 경우, 하나의 페이지만 수정하면 PC와 모바일 등 다양한 디바이스에서 동일하게 반영

### 반응형 웹의 핵심 기술

* 가변 그리드(fluid grid)
* 유연한 이미지(flexible images)
* 미디어 쿼리(media query)

### BreakPoint (중단점)

반응형 웹페이지의 작업 기준이 되는 중단점  
PC / Tablet / Mobile의 기준이 되는 규격 분기

<figure><img src="https://github.com/ShinjungOh/TIL/blob/main/Images/Media-Queries.png?raw=true" alt="Media-Queries" width="100%"></figure>

|    크기     |   BreakPoint    |
|:---------:|:---------------:|
|  Mobile   |    0 ~ 767px    |
|  Tablet   | 768px ~ 1923px  |
|    PC     | 1024px ~ 1439px |
| PC Large  |    1440px ~     |

Viewport의 크기 `PC > Tablet > Mobile`

* Viewport : 웹 브라우저를 통해 웹페이지에 접속했을 때, 주소창이나 탭 등을 제외하고 실제 웹페이지의 컨텐츠가 차지하는 영역

### 주의점

breakPoint를 많이 나누면 더 좋은 서비스를 만들 수 있지만, 개발하는 시간이 늘어나고 인건비가 증가  
웹 서비스를 기획 단계에서 프로젝트의 예산과 기간을 고려하여 BreakPoint를 몇 단계로 나눌 것인지 결정

> ✍️ **조각글**
>
> '반응형 웹'이라는 단어를 개발을 시작하기 전부터 회사에서 많이 들어봤는데 무슨 뜻인지 잘 몰랐다.
> 개발 입문 서적을 읽어보니 프론트엔드 개발이 어려운 이유로 반응형이 꼽히고 있었다.
> 스마트폰만 해도 기기의 종류가 천차만별이고 그 모든 화면 크기를 다 맞출 수는 없기 때문에 어렵다는 뜻이었다.
> 직접 개발을 해보니, 아무래도 구현을 할 때 여러 화면 크기를 생각해야 하기 때문에 번거롭고 복잡했다.
> 요새는 사람들이 주로 모바일 환경을 많이 이용하기 때문에 애초에 모바일 크기로만 구현해 놓은 사이트도 있었다.  

<br>

## 2. 디자인 시스템(Design System)

반응형 웹 디자인 이후에 등장한 개념  
디자인 시스템과 리액트 컴포넌트가 잘 맞아 떨어짐 

디자인 시스템은 UI 툴킷만을 지칭하는 것이 아니라 더 많은 것을 포함  
**프로젝트를 일관성이 있게 구축할 수 있도록** 디자인 원칙(Design Principles), 프로세스, UX 패턴, UI 툴킷,  코드 등 모든 것이 포함됨  

* 유형적인 요소 : UI 툴킷, 디자인 지침, 디자인 패턴 등
* 무형적인 요소(매우 추상적) : 브랜드 가치, 팀의 작업 방식, 사고 방식 등

### 개념 

[Laura Kalbag의 “Design Systems” 소개](https://24ways.org/2012/design-systems/)  
[Laura Kalbag의 “Design Systems” 슬라이드](https://speakerdeck.com/laurakalbag/design-systems-1)  
[Nielsen Norman Group의 “Design Systems 101”](https://www.nngroup.com/articles/design-systems-101/)

> 📌 Summary: A design system is a set of **standards** to manage design **at scale** by **reducing redundancy** 
> while creating a **shared language** and **visual consistency** across different pages and channels.
> 
> 디자인 시스템은 규모가 큰 디자인을 관리하는 표준의 집합  
> **공동으로 사용할 언어**(패턴)와, 여러 페이지와 채널에서 **시각적인 일관성**을 생성하는 과정에서 
> 중복을 줄임으로써, 규모에 맞게 디자인을 관리하기 위한 일련의 표준

> 📌 Definition: A design system is a complete set of standards intended 
> to manage design at scale using **reusable components and patterns**.
>
> 디자인 시스템은 **재사용 가능한 컴포넌트와 패턴**을 사용하여 규모에 맞게 디자인을 관리하기 위한 완전한 표준 세트  
> (패턴도 컴포넌트로 구현할 수 있음)

* Theme : primary color, secondary color, font 등을 포함하는 개념  

#### ⇒ Theme과 Component라는 개념을 활용할 수 있음 

### 장점

1. **이해 관계자 설득이 원활**   
   의사결정의 기준이 세워져 프로젝트 간접 참여자와도 소통이 쉬워짐

2. **프로젝트 참여자들의 의견 차이를 방지**   
   모든 팀의 목표를 통일할 수 있고 팀 간 소통이 자유로워짐

3. **사용자의 혼란 방지**   
   제품 및 브랜드의 일관성을 지속적으로 유지할 수 있기 때문에 사용자가 경험할 수 있는 혼란을 줄임

### UX(사용자 경험)

Jakob Nielsen과 Donald Norman은 UX 분야에서 전설적인 인물

- [제이콥 닐슨](https://ko.wikipedia.org/wiki/제이콥_닐슨) : 사용성 테스트로 유명
- [도널드 노먼](https://ko.wikipedia.org/wiki/도널드_노먼) : UX의 선구자 - 어포던스, 인터렉션 디자인
- [앨런 쿠퍼](https://en.wikipedia.org/wiki/Alan_Cooper) : 비주얼 베이직, 인터렉션 디자인

### 디자인 시스템 사례

일관된 방식으로 작동하도록 제공

* [Atlassian Design System](https://atlassian.design/) : Jira, Trello, Bitbucket, Sourcetree를 운영하는 회사
* [Material Design (Google)](https://material.io/)
* [Base Web (Uber)](https://baseweb.design/)
* [Polaris (Shopify)](https://polaris.shopify.com/)
* [Lightning Design System (Salesforce)](https://www.lightningdesignsystem.com/)
* [Mailchimp Pattern Library](https://ux.mailchimp.com/patterns) : 이메일 마케팅 서비스, 캐릭터의 성격도 지정되어 있음 
* [Ant Design](https://ant.design/) : [Figma](https://www.antforfigma.com/) 에서 사용 가능 

> ✍️ **조각글**
>
> 아틀라시안이라는 회사를 처음 들었는데 검색해보니 지라, 트렐로 등을 만든 회사여서 놀랐다.
> 직접 사용해본 적은 없지만 주변에서 사용하는 것을 보기도 했고, 트렐로 만들기 프로젝트도 해본 적이 있어서 무엇인지는 알고 있다.
> 강의 자료를 통해 디자인 시스템을 제공하는 다양한 사례를 볼 수 있었다. 이 중에는 Material Design만
> 알고 있었는데 다른 사이트들을 구경하며 비교할 수 있어 좋았다. 

### 디자인 시스템 툴

* [Figma](https://www.figma.com/)  
* [Sketch](https://www.sketch.com/)  
* [Framer](https://www.framer.com/) : 네이버, 토스
  * [토스의 프레이머 도입기](https://blog.toss.im/article/workflow-revolution)

### Gallery

[Design Systems Gallery](https://designsystemsrepo.com/design-systems/)  
[Design Systems](https://www.designsystems.com/open-design-systems/)

<br>

## 3. Atomic Design

[Atomic Design 소개 글](https://bradfrost.com/blog/post/atomic-web-design/)  

> <em>Atomic design is methodology for **creating design systems**.</em>  
> 아토믹 디자인은 디자인 시스템을 만드는 방법론이다.

디자인 시스템을 만드는 방법론 중 하나

<figure><img src="https://bradfrost.com/wp-content/uploads/2013/06/atomic-design.png" alt="atomic-design" width="100%"></figure>

세상은 매우 복잡하지만 몇 가지 원소의 조합으로 이루어진 것처럼,  
**원소(Atoms) → 분자(Molecules) → 유기체(Organisms) → 템플릿(Templates) → 페이지(Pages)**  
순서로 계층을 점진적으로 만들어 나가자는 방법 
