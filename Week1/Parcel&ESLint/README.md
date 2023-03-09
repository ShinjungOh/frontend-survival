# 5. Parcel & ESLint

## 1. Bundler

### 번들러

> 📦 **번들러**
>
> 의존성이 있는 모듈 코드를 하나(또는 여러 개)의 파일로 만들어주는 도구  
> 지금은 브라우저가 지원하는 ES Modules의 표준인 import, export 등이 예전에는 미지원  
> 파일을 하나로 합쳐주는 작업이 필요 ⇒ **번들링**  

### 빌드

빌드(build)는 소스 코드 파일을 컴퓨터나 휴대폰에서 실행할 수 있는 독립 소프트웨어 가공물로 변환하는 과정 또는 그 결과물    
코드 파일이 실행 코드로 변환되는 컴파일 과정이 핵심    

### Parcel

[Parcel 공식문서](https://parceljs.org/)  
[한국어 버전](https://ko.parceljs.org/getting_started.html)

The zero configuration build tool  
**Zero Configuration** : 특별한 설정 없이 바로 사용 가능한 빌드 도구

#### Parcel이 지원하는 것

- 최신 버전의 자바스크립트를 지원하지 않는 브라우저를 위해, 상위 버전의 JS를 옛날 버전으로 변환해주는 [babel](https://babeljs.io/)  
- 타입스크립트를 자바스크립트로 변환 

내부적으로 **SWC**를 사용해 기존 도구보다 빠름  

cf. ⚡️ ES Module, [esbuild](https://esbuild.github.io/) 를 적극 활용하는 [Vite](https://vitejs.dev/) 도 매우 빠름

### SWC

[SWC 공식문서](https://swc.rs/docs/getting-started)  
Rust로 만든 타입스크립트 컴파일러

### 해두면 좋은 세팅 

1. `package.json` 파일에 source 속성 추가

```
"source": "./index.html",
```

원래 `npx parcel index.html --port 8080`를 입력하면 원하는 포트로 서버를 열 수 있음  
매번 치기 번거롭기 때문에 `"source": "./index.html",` 설정을 추가  
⇒ `npx parcel --port 8080`만 입력해도 됨  

2. [parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) 패키지 설치 후 `.parcelrc` 파일 작성     

static 폴더의 파일을 정적 파일로 Serving 가능(이미지 등 Assets)   

```
// 설치
npm i -D parcel-reporter-static-files-copy
```

`.parcelrc` 파일
```json
{
  "extends": ["@parcel/config-default"],
  "reporters":  ["...", "parcel-reporter-static-files-copy"]
}
```

* 기본으로 사용하는 폴더 이름을 바꿀 수 있지만 `static` 으로 설정
* static 파일을 복사해서 처리해줌

### 빌드 + 정적 서버 실행

```
// package.json 파일의 scripts
// "build": "parcel build", ⇒ npm run build

npx parcel build
```

* ./dist 폴더가 생성됨
* ./dist를 실행 

```
npx servor ./dist
```

* 다시 개발하고 싶으면 폴더 삭제 후 다시 생성하면 됨  
* [servor](https://github.com/lukejacksonn/servor) : config가 없는 서버 

<br>

## 2. Lint

### 린트 

코드를 분석해 프로그램 오류, 버그, 스타일 오류 등에 표시를 달아놓기 위한 도구  
[정적 분석 도구](https://ko.wikipedia.org/wiki/정적_프로그램_분석) → 실행 없이 프로그램을 분석, 시간이 짧음 

cf. 동적 분석 : 실행 중인 프로그램을 분석하는 것 

> 🧶 **lint [명사]**   
> 1. 린트(붕대용으로 쓰는 부드러운 면직물의 일종)
> 2. (직물 직조 때 비어져 나온) 보푸라기실
> 3. 보풀  

### ESLint

[ESLint 공식문서](https://eslint.org/)

TSlint가 따로 있었지만 ESLint에 흡수됨 

### 장점

- 스타일 통일 → 코딩 컨벤션([Coding_conventions](https://en.wikipedia.org/wiki/Coding_conventions)) 유지 가능    
- 잠재적 문제 발견
- 베스트 프랙티스 추천 → 최신 트렌드를 학습하는데 활용 가능
  - Ex. `const { value } = data;`  

### 기본 세팅 

```json
{
    "editor.rulers": [
        80
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "trailing-spaces.trimOnSave": true
}
```

* `"source.fixAll.eslint": true` : JS/TS 파일을 저장할 때마다 ESLint를 실행하고 fix하도록 설정
* `"trailing-spaces.trimOnSave": true` : 불필요한 공백을 삭제

### 실행

`npm run lint && npm run check`   
린트 fix & 타입스크립트 컴파일 체크

#### package.json 파일의 scripts (관련 부분)

```
"scripts": {
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
  },
```
