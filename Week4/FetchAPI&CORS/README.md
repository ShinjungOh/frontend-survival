# 2. Fetch API & CORS

## 1. Fetch API

[Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)  
[Fetch 사용하기](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch)

웹 브라우저에서 사용하는 Web API   
네트워크 통신을 포함한 리소스 취득을 위한 인터페이스가 정의되어 있음

### 사용 방법

#### 기본적인 Fetch 요청

```js
fetch('http://example.com/movies.json')
  .then((response) => response.json())
  .then((data) => console.log(data));
```

#### json() 

[Response.json()](https://developer.mozilla.org/en-US/docs/Web/API/Response/json)

JSON 본문 콘텐츠를 추출하기 위해서는 json() 메소드를 호출해야 함   
json()은 응답 본문 텍스트를 JSON으로 파싱한 결과로 이행하는, 또 다른 프로미스를 반환

fetch를 사용해 JSON 구문분석하기  
API에서 데이터를 가져오는 가장 쉬운 방법    
`.json()` 메소드를 통해 JSON 응답을 JavaScript 객체 리터럴 또는 배열로 자동으로 구문분석

<br>

## 2. Promise

[Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)  

Promise 객체는 비동기 작업이 맞이할 미래의 **완료** 또는 **실패**와 그 **결과 값**을 나타냄

Promise는 프로미스가 생성된 시점에는 알려지지 않았을 수도 있는 값을 위한 대리자   
비동기 연산이 종료된 이후에 **결과 값과 실패 사유**를 처리하기 위한 처리기를 연결할 수 있음   
프로미스를 사용하면 비동기 메소드에서 **동기 메소드처럼 값을 반환**할 수 있음   
그렇지만 최종 결과를 반환하는 것이 아니고, 미래의 어떤 시점에 **결과를 제공하겠다는 약속(프로미스)을 반환**

### Promise의 상태 

Promise는 다음 중 하나의 상태를 가짐

* 대기(pending): 이행하지도, 거부하지도 않은 초기 상태
* 이행(fulfilled): 연산이 성공적으로 완료됨
* 거부(rejected): 연산이 실패함

<br>

## 3. ReadableStream

[ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)

Streams API의 ReadableStream 인터페이스는 바이트 데이터를 읽을 수 있는 스트림을 제공   
**Fetch AP**I는 Response 객체의 body 속성을 통해 ReadableStream의 구체적인 인스턴스를 제공

### 기본적인 사용법

ReadableStream을 처리하기 위해 우선 `getReader()`로 얻어온 후 진행

```js
fetch('http://localhost:3000/products');
// → Promise

await fetch('http://localhost:3000/products');
// → Response

const response = await fetch('http://localhost:3000/products');
// → response.body는 ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입
// → 원래는 chunk.done이 true일 때까지 반복해야 함

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

* `chunk.value`는 Uint8Array 타입이기 때문에 string으로 바꿔줘야 함
* [텍스트 디코더와 텍스트 인코더](https://ko.javascript.info/text-decoder)
* TextDecoder는 주어진 버퍼와 인코딩으로 값을 실제 자바스크립트 문자열로 읽을 수 있게 해줌

### JSON을 기본 지원

```js
const response = await fetch('http://localhost:3000/products');
const data = await response.json();
```

### 다른 HTTP Method를 쓰고 싶을 경우

```js
const response = fetch(url, {
    method: 'POST',
});
```

### 텍스트 파일 한 줄씩 처리하기

응답에서 읽어오는 데이터 청크(chunk)는 줄 단위로 깔끔하게 나뉘지 않으며, Uint8Array 형태(문자열 X)  
텍스트 파일을 가져와서 줄 단위로 처리하려면, '줄' 단위로 나누는 작업은 직접 구현해야 함

<br>

## 4. Unicode

[Unicode Consortium](https://home.unicode.org/)  
[유니코드](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C)  


유니코드(Unicode)는 전 세계의 모든 문자를 **컴퓨터에서 일관되게 표현**하고 다룰 수 있도록 설계된 산업 표준  
유니코드는 유니코드 협회(Unicode Consortium)가 제정  
이 표준에는 ISO 10646 문자 집합, 문자 인코딩, 문자 정보 데이터베이스, 문자들을 다루기 위한 알고리즘 등이 포함  

기존 인코딩의 규모나 범위 면에서 한정되어 있고, 다국어 환경에서는 서로 호환되지 않는 문제점을 해결

> 🌎 **ISO(국제 표준화 기구)**
> 
> International Organization for Standardization  
> 여러 나라의 표준 제정 단체들의 대표들로 이루어진 국제적인 표준화 기구  
> 나라마다 다른 산업, 통상 표준의 문제점을 해결하기 위해 **국제적으로 통용되는 표준을 개발하고 보급**  


> 🔠 **UCS(국제 문자 세트)**  
> 
> Universal Character Set, 범용 문자 집합    
> **ISO 10646**으로 정의된 문자 인코딩의 국제 표준

### Uint8Array

[Uint8Array](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)

Uint8Array 형식화 배열(TypedArray)  
플랫폼의 바이트 순서를 따르는 **8비트 부호 없는 정수의 배열**  
배열의 내용은 0으로 초기화됨  
배열이 생성되면 객체의 메소드를 사용하거나, 표준 배열 인덱스 구문(대괄호 표기법)을 사용하여 배열의 요소를 참조할 수 있음

<br>

## 5. CORS

### 동일 출처 정책(Same Origin Policy)

[동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)  

> [🔗 실습 링크](https://github.com/ShinjungOh/2023-learn-react/commit/95842f87e3d714f10a98e565bb40593b248a8088)

**웹 브라우저가 가진** 기본 보안 정책   
어떤 출처에서 불러온 문서나 스크립트가 **다른 출처**에서 가져온 리소스와 상호작용하는 것을 **제한**하는 보안 방식

> 🚨 **에러 메시지**   
> 
> <em>Access to fetch at 'http://localhost:3000/products' 
> from origin 'http://localhost:8080' has been blocked by CORS policy: 
> No 'Access-Control-Allow-Origin' header is present on the requested resource. 
> If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</em>  
> 
> 
> 개발자 도구의 네트워크 탭에서 살펴보면 서버에서 response 응답은 오지만(🟢 200 OK) 동일 출처가 아니기 때문에 브라우저에서 막힘 → 오류 발생    

웹 브라우저는 **Same Origin Policy**에 따라 웹 페이지와 리소스를 요청한 곳(여기서는 REST API 서버)이 서로 다른 출처(포트까지 포함)일 때, 서버에서 얻은 결과를 사용할 수 없게 막음  
⚠️ 서버에 요청하고 응답을 받아오는 것까지는 이미 진행이 다 된 상황

* 잠재적으로 해로울 수 있는 문서를 분리해, 공격받을 수 있는 경로를 줄여줌
* CORS를 사용해 교차 출처 접근을 허용할 수 있음

### 교차 출처 리소스 공유 (CORS)

[교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

Cross-Origin Resource Sharing  
CORS는 HTTP의 일부로, 어떤 호스트에서 자신의 콘텐츠를 불러갈 수 있는지 **서버에 지정**할 수 있는 방법  
출처가 다른 곳이어도 괜찮으며 리소스를 공유할 것이라고 **서버(리소스를 제공하는 곳)** 에서 알려줌  

* REST API 서버에서 `Headers`에 `Access-Control-Allow-Origin` 속성을 추가
* Express에서는 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html) 를 설치해서 사용

추가 HTTP 헤더를 사용해, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제  
웹 애플리케이션은 리소스가 **자신의 출처(도메인, 프로토콜, 포트)와 다를 때** 교차 출처 HTTP 요청을 실행

### 사용 방법 

#### 패키지 설치

```bash
npm i cors
npm i -D @types/cors
```

#### CORS 미들웨어 사용

```jsx
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());
```

