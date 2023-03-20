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
불필요한 데이터까지 전부 받아와서 처리해야 하는 REST API와 달리, 필요한 데이터만 지정해서 요청할 수 있기 때문에 비용이 절감 💸  
facebook, airbnb, github 등 유명한 사이트에서 사용 중  

* Graph 자료 구조
* Query에서 얻고자 하는 항목을 지정
* Graphql은 데이터 변경 유무에 따라 Query, Mutation을 구분
* Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)


> ❓**질의어(query language)**
> 
> [질의어](https://ko.wikipedia.org/wiki/%EC%A7%88%EC%9D%98%EC%96%B4)  
> 데이터베이스와 정보 시스템에 질의를 할 수 있게 하는 고급 컴퓨터 언어  
> Ex. GraphQL, SQL

### REST API vs GraphQL

#### 공통점

1. HTTP 통신 
   * HTTP : 텍스트, html을 전송하는 도구 -> 객체를 주고받을 수 없어서 텍스트로 표기 (JSON)
   * JSON : 자바스크립트 객체 표기법

2. 구성
   * 헤더 : 요약 정보 (✉️편지봉투)
   * 보내는 이, 보내는 내용 형태
   * 바디 : 주고받은 객체가 들어있음

#### 차이점

|         차이점         |               REST                |                   GRAPHQL                     |
|:-------------------:|:---------------------------------:|:---------------------------------------------:|
|     대규모 접속자 처리      |                어려움                |                      쉬움                       |
|         형태          | 주소<br/>https://naver.com/board/1  |                함수<br/>board(1)                |
|       응답 데이터        |              모든 데이터               |          필요한 데이터만 골라서 응답 -> **비용절감**          |
| API에 전송을 요청하는 라이브러리 |               axios               |                 apollo-client                 |
|       API 테스트       | 포스트맨<br/>https://www.postman.com/ | 플레이그라운드<br/>https://www.graphqlbin.com/v2/new |
|       API 명세서       |    스웨거<br/>https://swagger.io/    | 플레이그라운드<br/>https://www.graphqlbin.com/v2/new |

* Graphql은 테스트와 명세서가 합쳐져 있어서 편리

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
* 프로그래밍 언어와 플랫폼에 독립적 -> 서로 다른 시스템 간 객체를 교환하기 용이 (언어가 달라도 소통 가능)
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

|        |       JSON        |         XML          |
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

## 7. SRP(단일 책임 원칙)


<br>

## 8. Atomic Design

<br>

## 9. React component 와 props
