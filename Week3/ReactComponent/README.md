# 1. React Component

## 1. Thinking in React

> 📌 [Thinking in React](https://react.dev/learn/thinking-in-react)  
> 리액트를 이용하는 기초

### Start with the mockup

* JSON API 
* 디자이너의 목업 - 최종 결과물은 아니지만 뼈대가 되는 틀

```bash
# JSON API가 반환하는 예시 데이터 형태
 
[
  { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
  { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
  { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
  { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
  { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
  { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
]
```

BE에서 JSON 형태의 데이터를 돌려주는 API를 제공한다고 가정(대부분은 REST API 또는 GraphQL)  
FE는 이 데이터를 사용자가 볼 수 있도록 UI를 구성     
React는 **선언형**(HTML과 유사한 모양의 DSL을 사용)으로 UI를 구성할 수 있음

### Step 1: Break the UI into a component hierarchy

UI를 컴포넌트 계층 구조로 나누기

### Step 2: Build a static version in React

React에서 정적 버전 빌드

<br>

## 2. REST API 와 GraphQL

### REST API란 무엇인가?

* GET, POST, PUT/PATCH, DELETE (CRUD - 기본적인 데이터 처리 기능)
* Resource 중심으로 조작 

### GraphQL은 왜 등장했는가?

[GraphQL](https://graphql.org/)  
[raphQL is the better REST](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)  
[GraphQL Playground](https://github.com/graphql/graphql-playground)  

페이스북 개발 - 페이스북에서 발생하는 수많은 데이터를 처리하기 위함  
불필요한 데이터까지 전부 받아와서 처리해야 하는 REST API와 달리, 필요한 데이터만 지정해서 요청할 수 있기 때문에 **비용이 절감** 💸  
facebook, airbnb, github 등 유명한 사이트에서 사용 중  

* Graph 자료 구조
* Query에서 얻고자 하는 항목을 지정
* Graphql은 데이터 변경 유무에 따라 Query, Mutation을 구분
* Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)


> ❓ **질의어(query language)**
> 
> [질의어](https://ko.wikipedia.org/wiki/%EC%A7%88%EC%9D%98%EC%96%B4)  
> 데이터베이스와 정보 시스템에 질의를 할 수 있게 하는 고급 컴퓨터 언어  
> Ex. GraphQL, SQL

### REST API vs GraphQL

#### 공통점

1. HTTP 통신 
   * HTTP : 텍스트, html을 전송하는 도구 → 객체를 주고받을 수 없어서 텍스트로 표기 (JSON)
   * JSON : 자바스크립트 객체 표기법

2. 구성
   * 헤더 : 요약 정보 (✉️편지봉투)
   * 보내는 이, 보내는 내용 형태
   * 바디 : 주고받은 객체가 들어있음

#### 차이점

|         차이점         |               REST               |                   GRAPHQL                    |
|:-------------------:|:--------------------------------:|:--------------------------------------------:|
|     대규모 접속자 처리      |               어려움                |                      쉬움                      |
|         형태          | 주소<br/>https://naver.com/board/1 |               함수<br/>board(1)                |
|       응답 데이터        |              모든 데이터              |         필요한 데이터만 골라서 응답 -> **비용절감**          |
| API에 전송을 요청하는 라이브러리 |              axios               |                apollo-client                 |
|       API 테스트       | [포스트맨](https://www.postman.com/) | [플레이그라운드](https://www.graphqlbin.com/v2/new) |
|       API 명세서       |    [스웨거](https://swagger.io/)    | [플레이그라운드](https://www.graphqlbin.com/v2/new) |

* GraphQL은 테스트와 명세서가 합쳐져 있어서 편리

<br>

## 3. JSON

### JSON

[JSON(JavaScript Object Notation)](https://ko.wikipedia.org/wiki/JSON)  

Javascript 객체 문법으로 구조화된 데이터를 표현하기 위한 표준 포맷  
데이터를 나타내는 포맷 중 하나  
자바스크립트와의 호환성이 좋음  
더글라스 크록포드가 만듦   
기본 데이터 타입인 문자열, 숫자, 배열, 불리언, 다른 객체, null 포함 가능

#### 장점 

* 텍스트로 구성되어, 사람과 컴퓨터 모두 읽고 쓰기 쉬움
* 프로그래밍 언어와 플랫폼에 독립적 → 서로 다른 시스템 간 객체를 교환하기 용이 (언어가 달라도 소통 가능)
* api, config 파일에 활용
* 가벼움

#### 주의점

* key - value 만 담을 수 있음
* "큰 따옴표만 사용"
* undefined 불가
* 메소드 불가

### XML

[XML(Extensible Markup Language)](https://ko.wikipedia.org/wiki/XML)  

데이터를 정의하는 규칙을 제공하는 마크업 언어  
데이터를 나타내는 포맷 중 하나  
<> 태그 기반 </>  

### JSON vs XML

* XML이 더 무거움 (글자 수가 많음)
* XML은 자바스크립트 오브젝트로 변환하기 복잡
  * JSON의 경우, JSON.parse 이용
* XML의 태그는 이름이 정해져 있지 않음 (HTML과의 차이점)

|  차이점   |       JSON        |         XML          |
|:------:|:-----------------:|:--------------------:|
|   무게   |        가벼움        |         무거움          |
| JS 호환성 | 좋음<br/>JSON.parse | 나쁨<br/>오브젝트로 변환하기 복잡 |
| 태그 이름  |    이름이 정해져 있음     |    이름이 정해져 있지 않음     |

<br>

## 4. DSL(Domain-Specific Language)

[DSL](https://ko.wikipedia.org/wiki/%EB%8F%84%EB%A9%94%EC%9D%B8_%ED%8A%B9%ED%99%94_%EC%96%B8%EC%96%B4)  

도메인 특화 언어(Domain-specific language)      
특정한 도메인을 적용하는데 특화된 컴퓨터 언어  
Ex. HTML(Web)  

FE는 BE에서 넘겨준 데이터를 사용자가 볼 수 있도록 UI를 구성       
React는 **선언형**(HTML과 유사한 모양의 DSL을 사용)으로 UI를 구성할 수 있음

> 💡 선언형으로 만들 수 있음 → 내부 내용이 바뀌었을 때 자동으로 업데이트 

<br>

## 5. 선언형 프로그래밍

[선언형 프로그래밍](https://ko.wikipedia.org/wiki/%EC%84%A0%EC%96%B8%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

* 바로 목적 서술, 목표를 명시 → 예측 가능하고 디버깅 용이  
* Ex. React

✅ **React의 특징 중 하나**  
React는 선언적 API를 제공하기 때문에 갱신이 될 때마다 매번 무엇이 바뀌었는지를 걱정할 필요가 없음  
일일이 변화된 부분을 지정해 바꿔주는 것이 아니라, 그렇게 되도록 선언   
⇒ 컴포넌트의 갱신이 예측 가능해지고, 빠른 앱을 만들 수 있음  

> **React의 선언형 프로그래밍**
> 
> JSX는 React.createElement를 사용하도록 자바스트립트 코드를 변환  
> 변환된 자바스크립트 코드는 **Virtual DOM** 트리를 바꿔주고, VDOM과 화면을 비교해서 바뀐 부분만 업데이트하는 **재조정** 과정이 발생  
> React 내부에서는 비교 (diffing) 알고리즘을 통해 무엇이 변경되었는지 계산이 일어남  
> 이러한 접근방식을 통해 **React의 선언적 API**가 가능해짐

<br>

## 6. 명령형 프로그래밍

[명령형 프로그래밍](https://ko.wikipedia.org/wiki/%EB%AA%85%EB%A0%B9%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

* 절차를 일일이 나열, 실행할 알고리즘을 명시
* Ex. jQuery, C, JAVA

<br>

## 7. React component 와 props

### Component-Based

✅ **React의 특징 중 하나**

> <em>Build encapsulated components that manage their own state, then **compose** them to make **complex UIs**.   
> 스스로 상태를 관리하는 캡슐화된 컴포넌트를 만드세요. 그리고 이를 **조합**해 **복잡한 UI**를 만들어보세요.</em>  
⇒ **간단한 컴포넌트를 모아서 복잡한 UI를 조립**

작은 컴포넌트=부품을 만들어서 조립  
조합은 가지수를 폭발적으로 늘릴 수 있는 가장 전형적인 방법  
⚙️ 조합하는 방식에 따라 적은 부품만 가지고도 다양하게 만들 수 있음

### 컴포넌트를 나누는 기준

1. SRP (Single Responsibility Principle)
   * 컴포넌트가 너무 커질 경우 분리가 필요 (책임이 많고, 코드가 길어질 때)
2. CSS
   * 이미 알고 있는 기준을 재활용
3. Design’s Layer (디자인 측면)
   * 레이어도 트리 형태로 이루어짐
4. Information Architecture (JSON Schema의 영향)
   * 실제로 엄청 많이 사용
   * 자연스러운 SRP를 위해서 사실상 강제됨

### Extract Function

> 🛠 **마틴 파울러 <리팩토링>**
>
> 리팩토링에서 쓸 수 있는 기법
> * [Extract Function](https://refactoring.com/catalog/extractFunction.html)
> * [Inline Function](https://refactoring.com/catalog/inlineFunction.html)

함수 추출은 SRP를 위해 자주 쓰이는 방법 중 하나   
변화의 크기(영향 범위)를 제약

* 일단 길게 코드를 작성하고, 적절히 자를 수 있는 부분이 보일 때 **함수로 추출**
* 코드를 작성하기 어려운 상황에 직면했을 때 **함수로 추출**

💡 바로 다른 파일을 만들어야 한다고 생각하지 않아도 됨  
컴포넌트를 나누는 기준이 애매하면 다시 **하나의 컴포넌트로 합쳤다가(Inline Method)** 다시 나눠줘도 됨

### Props

[Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)    
[Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)

> <em>모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 **순수 함수**처럼 동작해야 한다.</em>

나눠진 컴포넌트를 서로 연결하는 방법  
컴포넌트의 자체 props를 수정하면 안됨  
⚠️ TypeScript를 잘 쓰거나 잘못 쓰게 되는 포인트 중 하나  
적절한 균형점을 잡는 것이 중요

> ❄️ **순수 함수**
>
> * 동일한 Input을 받았을 때, 항상 동일한 Output을 반환하는 함수
> * 함수 사용 시, 함수 내에 사이드이펙트가 없어야 함

<br>

## 8. SRP(단일 책임 원칙)

[SRP (Single Responsibility Principle)](https://ko.wikipedia.org/wiki/단일_책임_원칙)

객체 지향 프로그래밍에서 단일 책임 원칙이란 모든 클래스(React는 컴포넌트)는 하나의 책임만 가지며, 클래스는 그 책임을 완전히 캡슐화해야 한다는 의미  
한 클래스를 한 관심사에 집중하도록 유지하는 것은 클래스를 더욱 튼튼하게 만듦  

> <em>책임(Responsibility)을 변경하려는 이유로 정의하고, 어떤 클래스나 모듈은 변경하려는 **단 하나 이유**만을 가져야 한다.</em>

* 클래스 : 객체 지향 프로그래밍을 지원하는 언어에서 컴포넌트를 구현할 수 있는 기술 중 하나 

### 관심사 분리

[관심사 분리](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%8B%AC%EC%82%AC_%EB%B6%84%EB%A6%AC)

관심사 분리(separation of concerns, SoC)는 컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙  
**관심사**란 프로그램 코드에 영향을 미치는 정보의 집합이며, 각 부문은 개개의 관심사를 해결  
**관심사의 분리**는 정보를 잘 정의된 인터페이스가 있는 코드 부분 안에 **캡슐화**(정보 숨기기의 한 수단) 시킴으로써 달성  
**계층화**된 디자인은 관심사 분리의 다른 구현 (Ex. 표현 계층, 비즈니스 로직 계층, 데이터 접근 계층, 퍼시스턴스 계층)    
관심사 분리는 추상화의 일종 

#### 장점 

* 코드의 단순화 및 유지보수가 용이 (재사용하기 좋음)      
* 다른 부분의 세세한 사항을 모르더라도, 하나의 관심사의 코드 부분을 개선하거나 수정할 수 있음

<br>

## 9. Atomic Design

[Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)

우리가 잘 알고 있는 계층형 구조를 몇 가지 카테고리로 묶은 방법

* 이대로 해야 한다가 아닌, 우리가 기존에 사용하고 있는 방법  
* 개념적인 것
* 원소들을 가지고 세상을 구성하듯 컴포넌트를 잘 조합해 원하는 것을 만들자는 의미
* 컴포넌트를 잘게 나눌수록 atom에 가까워짐 

<img src="https://bradfrost.com/wp-content/uploads/2013/06/atomic-design.png" width="800">
