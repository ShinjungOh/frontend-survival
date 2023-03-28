# 2. Fetch API & CORS

## 1. Fetch API

[Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)  
[Fetch 사용하기](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch)

웹 브라우저에서 사용하는 Web API   
네트워크 통신을 포함한 리소스 취득을 위한 인터페이스가 정의되어 있음 

<br>

## 2. Promise

[Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)  

Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냄

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

<br>

## 5. CORS

### 동일 출처 정책(Same Origin Policy)

[동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)  

**웹 브라우저가 가진** 기본 보안 정책 

> 개발자 도구의 네트워크 탭에서 살펴보면 서버에서 response 응답은 오지만(🟢 200 OK) 동일 출처가 아니기 때문에 브라우저에서 막는다. → 오류 발생    

웹 브라우저는 **Same Origin Policy**에 따라 웹 페이지와 리소스를 요청한 곳(여기서는 REST API 서버)이 서로 다른 출처(포트까지 포함)일 때, 
서버에서 얻은 결과를 사용할 수 없게 막음  
⚠️ 서버에 요청하고 응답을 받아오는 것까지는 이미 진행이 다 된 상황

### 교차 출처 리소스 공유 (CORS)

[교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

출처가 다른 곳이어도 괜찮으며 리소스를 공유할 것이라고 **서버(리소스를 제공하는 곳)** 에서 알려줌  
* REST API 서버에서 `Headers`에 `Access-Control-Allow-Origin` 속성을 추가
* Express에서는 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html) 를 설치해서 사용

### 패키지 설치

```bash
npm i cors
npm i -D @types/cors
```

### CORS 미들웨어 사용

```jsx
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());
```

