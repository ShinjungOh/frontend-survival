# 1-1. 개발 환경

## 개발 환경 세팅

### Node.js ✅

대부분의 개발자가 Node.js를 기반으로 사용하는데, 세팅하기 까다롭고 어려움
    
🚀 **왜 어려운가?** → 고정적이고 오래가는 도구가 있는 것이 아니라 **계속 바뀌기 때문**  
전체적인 흐름을 파악하고 앞으로의 변경에 대응할 수 있는 능력을 키우는 것이 중요  

### Deno(디노) 

Node.js 개발자가 새로 시작한 프로젝트 
  * 훨씬 간단하지만, 사용하는 곳이 별로 없음 
  * 기술의 최전선, 도전적인 느낌
  * https://deno.land/

<br><br>

## JavaScript 개발 환경 (Node.js) 세팅

* [Node.js](https://nodejs.org/ko/)  
  * 최신 버전인지 확인 `node -v`   
  ⇒ 23.03.06 현재 기준 18.14.2 LTS
  * 무조건 최신 버전을 고집할 필요는 없음 
* [fnm (Fast Node Manager)](https://fnm.vercel.app/)
  * nvm보다 빠름

```
// Homebrew를 이용해 fnm을 설치
brew install fnm


// Node.js LTS(Long Term Support) 버전을 설치
fnm install --lts
```

<br><br>

## TypeScript + React + Jest + ESLint + Parcel 개발 환경 세팅

### 1. 작업 폴더를 준비

```
mkdir my-app

cd my-app
```

<br>

### 2. IDE 실행

CLI로 열거나 직접 열어서 폴더를 가져오거나 상관 없음

```
// Visual Studio Code 열기

code .
```

<br>

### 3. npm 패키지를 준비

```
npm init -y
```

* -y : npm init에 무조건 yes로 대답한다는 옵션 

<br>

![](../../images/npm_package.png)

* name: `소문자-이름` 형태
* version: 시맨틱 버전(Semantic Versioning)
* scripts: 실행할 수 있는 명령어, 직접 커스텀 가능 

<br>

### 4. `.gitignore` 파일을 작성  

⚠️ 최소한 `node_modules`를 통째로 커밋하는 일을 미연에 방지할 것 

* gitignore 생성 사이트 : https://www.toptal.com/developers/gitignore
  * node를 검색하면 맞춤으로 생성
* github에서 기본 제공하는 gitignore : https://github.com/github/gitignore
  * 언어별 선택 가능
    * [Node.gitignore](https://github.com/github/gitignore/blob/main/Node.gitignore)

<br>

### 5. 타입스크립트 설정

```
// 타입스크립트 설치
npm i -D typescript
```

* 기본 설치 시 : dependencies로 추가, 프로그램에서 직접 사용하는 것 
* i : install
* -D : devdependencies로 추가하는 옵션, 개발 환경에서만 사용(도구로서 사용됨)
  * --save-dev 옵션과 동일

```
// 타입스크립트 컴파일러를 실행해 tsconfig.json 파일 생성 
npx tsc --init
```

* npx : 실행하려는 명령을 수행 
* tsc : 타입스크립트 컴파일러 
* --init : 초기화 

<br>

### 6. `tsconfig.json` 파일의 jsx 속성을 변경

"jsx"의 주석을 해제하고, "jsx": "react-jsx"로 설정 변경  

[tsconfig 설정 참고 사이트](https://www.typescriptlang.org/tsconfig)

<br>

### 7. ESLint 설정

```
// ESLint 설치
npm i -D eslint


// eslint를 실행해 .eslintrc.js 파일 생성
npx eslint --init
```

<br>

### 8. `.eslintrc.js` 파일을 적절히 수정

.eslintrc.js에서 `.js`는 생략 가능하며, 없을 시 json 형식   
아직 Jest를 설치하지 않았지만, 여기서 미리 env에 `jest: true`를 잡아주면 좋음  

<br>

### 9. `.eslintignore` 파일을 작성

eslint를 적용시키지 않을 파일을 추가    
gitignore를 복사해서 .eslintignore에 넣어줘도 됨  

<br>

### 10. 리액트 설치

```
// 리액트 설치
npm i react react-dom


// 타입스크립트 관련 설치
npm i -D @types/react @types/react-dom
```

@types : 실제 배포될 때 사용되는 것이 아니라, 개발 환경에서 도구처럼 쓰이기 때문에 -D로 devdependencies에 추가  

<br>

### 11. 테스팅 도구 설치

```
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom
```

* swc 혹은 테스팅 라이브러리 쓰는 방법이 바뀌면 설치 명령이 조율될 수 있음
* jest와 swc를 같이 사용하는 것이 핵심

<br>

### 12. `jest.config.js` 파일을 생성해 테스트에서 SWC를 사용

[참고](https://github.com/ahastudio/CodingLife/blob/main/20220726/react/jest.config.js)

```js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: [
    '@testing-library/jest-dom/extend-expect',
  ],
  transform: {
    '^.+\\.(t|j)sx?$': ['@swc/jest', {
      jsc: {
        parser: {
          syntax: 'typescript',
          jsx: true,
          decorators: true,
        },
        transform: {
          react: {
            runtime: 'automatic',
          },
        },
      },
    }],
  },
  testPathIgnorePatterns: [
    '<rootDir>/node_modules/',
    '<rootDir>/dist/',
  ],
};
```

* transform : 타입스크립트 코드는 그냥 실행할 수 없기 때문에 자바스크립트로 변환을 해야 함

<br>

### 13. Parcel 설치

parcel을 이용해 웹서버, dev server 등을 띄울 수 있음 

```
npm i -D parcel
```

<br>

### 14. `package.json` 파일의 scripts를 적절히 수정한다.

[참고](https://github.com/ahastudio/CodingLife/blob/main/20220726/react/package.json)

```
"scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
```

* build: dist 폴더 생성 
* check: 컴파일 체크 
* coverage: jest --coverage --coverage-reporters html,
* watch:test: jest --watchAll

> ⚠️ `"source": "index.html",`를 추가해야 웹 서버가 실행됨    
> node의 경우 `"main": "index.js"`으로 되어있음

<br>

### 15. 기본 코드 작성

* `index.html`
* `src/main.tsx`
* `src/App.tsx`
* `src/App.test.tsx`
* `src/components/Greeting.test.tsx`
* `src/components/Greeting.tsx`
