# 1. Express

## 1. Express

[Express](https://expressjs.com/ko/)

Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크  
Node.js 초기부터 존재한 오래된 프레임워크 → 표준적  
웹 애플리케이션, API 개발을 위해 설계됨  

### 프론트엔드도 백엔드 지식을 알아야 할까?
 
백엔드 지식 없이 프론트 작업을 하는 것은 불가능  

> 👩🏻‍💻 **백엔드 지식을 알아야 하는 이유** 
> 1. 백엔드와 협업   
> 이슈가 발생했을 때 어디쪽에서 발생했는지 확실히 알 수 있음(억울한 상황 방지)
> 2. BFF(Backend For Frontend)  
>  API를 다이렉트로 의존할 때의 이슈들을 해결하고자 BFF가 등장  
>  프론트엔드를 위한 중간 서버를 구현하는 것
> 3. 테스트에서의 모킹
> 4. Nextjs 사용 시 API를 따로 잡아줄 때 

### 간단한 서버 앱 npm 패키지 세팅

#### 1. 작업 폴더 준비

```bash
mkdir express-demo-app
cd mkdir express-demo-app
```

#### 2. 패키지 초기화

```bash
npm init -y
```

#### 3. gitignore 파일 생성

```bash
touch .gitignore
echo "/node_modules/" > .gitignore
```

#### 4. TypeScript

```bash
npm i -D typescript
npx tsc --init
```

#### 5. ts-node 설치

컴파일 없이 Node.js에서 타입스크립트를 직접 실행할 수 있음(개발 환경에서 TS 코드 테스트 가능)

```bash
npm i -D ts-node
```

#### 6. ESLint

```bash
npm i -D eslint
npx eslint --init
```

```
? How would you like to use ESLint? …
❯ To check syntax, find problems, and enforce code style

? What type of modules does your project use? …
❯ JavaScript modules (import/export)

? Which framework does your project use? …
❯ None of these ✅

? Does your project use TypeScript?
❯ Yes

? Where does your code run? …
✔ Node ✅

? How would you like to define a style for your project? …
❯ Use a popular style guide

? Which style guide do you want to follow? …
❯ XO: https://github.com/xojs/eslint-config-xo-typescript

? What format do you want your config file to be in? …
❯ JavaScript

? Would you like to install them now with npm?
❯ Yes

? Which package manager do you want to use? … 
❯ npm
```

#### 7. Express 설치

```bash
npm i express
npm i -D @types/express
```

### Hello World 예제

[Express 예제](https://expressjs.com/ko/starter/hello-world.html)

#### 1. TypeScript에 맞춰서 app.ts 파일 작성

```jsx
// app.ts
import express from 'express';

const port = 3000;

const app = express();

app.get('/', (req, res) => {
    res.send('Hello, world!');
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
});
```

#### 2. ts-node로 실행

```bash
npx ts-node app.ts
```

#### 3. 코드를 수정할 때마다 서버를 재실행해야 하는 문제를 피하기 위해 [nodemon](https://github.com/remy/nodemon) 사용

```bash
npm i -D nodemon
npx nodemon app.ts
```

⚠️ ts-node에 대한 의존성이 있기 때문에 ts-node를 꼭 설치해야 함   
⚠️ 실제 서비스하는 서버에서 사용하면 안 됨  

> <em>**Monitor** for any changes in your node.js application 
> and **automatically restart the server** - perfect for development</em>  
> node.js 애플리케이션의 변경 사항을 모니터링하고 서버를 자동으로 다시 시작합니다. 개발에 적합합니다.

#### 4. package.json 파일 scripts 추가

```
 "scripts": {
    "start": "nodemon app.ts",
    "lint": "eslint --fix ."
  },
```

<br>

## 2. URL 구조

[Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)

대개는 `필딩 제약 조건` 4가지를 모두 만족하지 않고, **Resource**와 **HTTP Verb**만 도입하는 수준으로 사용

- ❌ /write-post
- ✅ /posts → 뭔가를 한다 (CRUD)

<figure><img src="https://martinfowler.com/articles/images/richardsonMaturityModel/overview.png" alt="richardsonMaturityModel" width="100%"></figure>

<br>

## 3. REST API

기본 리소스 URL: /products



Thinking in React 예제

```js
app.get('/products', (req, res) => {

	const products = [
		{
			category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
		},
		{
			category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit',
		},
		{
			category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit',
		},
		{
			category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach',
		},
		{
			category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin',
		},
		{
			category: 'Vegetables', price: '$1', stocked: true, name: 'Peas',
		},
	];
	
	res.send({ products });
});
```

<br>

## 4. HTTP Method(CRUD)

> 📌 Create(생성), Read(상세 조회), Update(수정), Delete(삭제) + 목록 조회  

기본적인 데이터 처리 기능  
하나의 기능을 만들 때 최소한 5개의 API가 있어야 함 

CRUD에 대해 **HTTP Method**를 대입  
Read는 Collection(복수)과 Item(Element)(단수)로 나뉨  

1. **R**ead (Collection) → `GET` /products ⇒ 상품 목록 확인
2. **R**ead (Item) → `GET` /products/{id} ⇒ 특정 상품 정보 확인
3. **C**reate (Collection Pattern 활용) → `POST` /products ⇒ 상품 추가 (JSON 정보 함께 전달)
4. **U**pdate (Item) → `PUT(덮어쓰기) 또는 PATCH(일부 변경)` /products/{id} ⇒ 특정 상품 정보 변경 (JSON 정보 함께 전달)
5. **D**elete (Item) → `DELETE` /products/{id} ⇒ 특정 상품 삭제

PUT, PATCH, DELETE는 fetch API를 통해 AJAX 요청으로만 처리 가능(브라우저 기본 동작이 아님) 
