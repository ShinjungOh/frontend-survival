# 5. Parcel & ESLint

## 1. Bundler

### ๋ฒ๋ค๋ฌ

> ๐ฆ **๋ฒ๋ค๋ฌ**
>
> ์์กด์ฑ์ด ์๋ ๋ชจ๋ ์ฝ๋๋ฅผ ํ๋(๋๋ ์ฌ๋ฌ ๊ฐ)์ ํ์ผ๋ก ๋ง๋ค์ด์ฃผ๋ ๋๊ตฌ  
> ์ง๊ธ์ ๋ธ๋ผ์ฐ์ ๊ฐ ์ง์ํ๋ ES Modules์ ํ์ค์ธ import, export ๋ฑ์ด ์์ ์๋ ๋ฏธ์ง์  
> ํ์ผ์ ํ๋๋ก ํฉ์ณ์ฃผ๋ ์์์ด ํ์ โ **๋ฒ๋ค๋ง**  

### ๋น๋

๋น๋(build)๋ ์์ค ์ฝ๋ ํ์ผ์ ์ปดํจํฐ๋ ํด๋ํฐ์์ ์คํํ  ์ ์๋ ๋๋ฆฝ ์ํํธ์จ์ด ๊ฐ๊ณต๋ฌผ๋ก ๋ณํํ๋ ๊ณผ์  ๋๋ ๊ทธ ๊ฒฐ๊ณผ๋ฌผ    
์ฝ๋ ํ์ผ์ด ์คํ ์ฝ๋๋ก ๋ณํ๋๋ ์ปดํ์ผ ๊ณผ์ ์ด ํต์ฌ    

### Parcel

[Parcel ๊ณต์๋ฌธ์](https://parceljs.org/)  
[ํ๊ตญ์ด ๋ฒ์ ](https://ko.parceljs.org/getting_started.html)

The zero configuration build tool  
**Zero Configuration** : ํน๋ณํ ์ค์  ์์ด ๋ฐ๋ก ์ฌ์ฉ ๊ฐ๋ฅํ ๋น๋ ๋๊ตฌ

#### Parcel์ด ์ง์ํ๋ ๊ฒ

- ์ต์  ๋ฒ์ ์ ์๋ฐ์คํฌ๋ฆฝํธ๋ฅผ ์ง์ํ์ง ์๋ ๋ธ๋ผ์ฐ์ ๋ฅผ ์ํด, ์์ ๋ฒ์ ์ JS๋ฅผ ์๋  ๋ฒ์ ์ผ๋ก ๋ณํํด์ฃผ๋ [babel](https://babeljs.io/)  
- ํ์์คํฌ๋ฆฝํธ๋ฅผ ์๋ฐ์คํฌ๋ฆฝํธ๋ก ๋ณํ 

๋ด๋ถ์ ์ผ๋ก **SWC**๋ฅผ ์ฌ์ฉํด ๊ธฐ์กด ๋๊ตฌ๋ณด๋ค ๋น ๋ฆ  

cf. โก๏ธ ES Module, [esbuild](https://esbuild.github.io/) ๋ฅผ ์ ๊ทน ํ์ฉํ๋ [Vite](https://vitejs.dev/) ๋ ๋งค์ฐ ๋น ๋ฆ

### SWC

[SWC ๊ณต์๋ฌธ์](https://swc.rs/docs/getting-started)  
Rust๋ก ๋ง๋  ํ์์คํฌ๋ฆฝํธ ์ปดํ์ผ๋ฌ

### ํด๋๋ฉด ์ข์ ์ธํ 

1. `package.json` ํ์ผ์ source ์์ฑ ์ถ๊ฐ

```
"source": "./index.html",
```

์๋ `npx parcel index.html --port 8080`๋ฅผ ์๋ ฅํ๋ฉด ์ํ๋ ํฌํธ๋ก ์๋ฒ๋ฅผ ์ด ์ ์์  
๋งค๋ฒ ์น๊ธฐ ๋ฒ๊ฑฐ๋กญ๊ธฐ ๋๋ฌธ์ `"source": "./index.html",` ์ค์ ์ ์ถ๊ฐ  
โ `npx parcel --port 8080`๋ง ์๋ ฅํด๋ ๋จ  

2. [parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) ํจํค์ง ์ค์น ํ `.parcelrc` ํ์ผ ์์ฑ     

static ํด๋์ ํ์ผ์ ์ ์  ํ์ผ๋ก Serving ๊ฐ๋ฅ(์ด๋ฏธ์ง ๋ฑ Assets)   

```
// ์ค์น
npm i -D parcel-reporter-static-files-copy
```

`.parcelrc` ํ์ผ
```json
{
  "extends": ["@parcel/config-default"],
  "reporters":  ["...", "parcel-reporter-static-files-copy"]
}
```

* ๊ธฐ๋ณธ์ผ๋ก ์ฌ์ฉํ๋ ํด๋ ์ด๋ฆ์ ๋ฐ๊ฟ ์ ์์ง๋ง `static` ์ผ๋ก ์ค์ 
* static ํ์ผ์ ๋ณต์ฌํด์ ์ฒ๋ฆฌํด์ค

### ๋น๋ + ์ ์  ์๋ฒ ์คํ

```
// package.json ํ์ผ์ scripts
// "build": "parcel build", โ npm run build

npx parcel build
```

* ./dist ํด๋๊ฐ ์์ฑ๋จ
* ./dist๋ฅผ ์คํ 

```
npx servor ./dist
```

* ๋ค์ ๊ฐ๋ฐํ๊ณ  ์ถ์ผ๋ฉด ํด๋ ์ญ์  ํ ๋ค์ ์์ฑํ๋ฉด ๋จ  
* [servor](https://github.com/lukejacksonn/servor) : config๊ฐ ์๋ ์๋ฒ 

<br>

## 2. Lint

### ๋ฆฐํธ 

์ฝ๋๋ฅผ ๋ถ์ํด ํ๋ก๊ทธ๋จ ์ค๋ฅ, ๋ฒ๊ทธ, ์คํ์ผ ์ค๋ฅ ๋ฑ์ ํ์๋ฅผ ๋ฌ์๋๊ธฐ ์ํ ๋๊ตฌ  
[์ ์  ๋ถ์ ๋๊ตฌ](https://ko.wikipedia.org/wiki/์ ์ _ํ๋ก๊ทธ๋จ_๋ถ์) โ ์คํ ์์ด ํ๋ก๊ทธ๋จ์ ๋ถ์, ์๊ฐ์ด ์งง์ 

cf. ๋์  ๋ถ์ : ์คํ ์ค์ธ ํ๋ก๊ทธ๋จ์ ๋ถ์ํ๋ ๊ฒ 

> ๐งถ **lint [๋ช์ฌ]**   
> 1. ๋ฆฐํธ(๋ถ๋์ฉ์ผ๋ก ์ฐ๋ ๋ถ๋๋ฌ์ด ๋ฉด์ง๋ฌผ์ ์ผ์ข)
> 2. (์ง๋ฌผ ์ง์กฐ ๋ ๋น์ด์ ธ ๋์จ) ๋ณดํธ๋ผ๊ธฐ์ค
> 3. ๋ณดํ  

### ESLint

[ESLint ๊ณต์๋ฌธ์](https://eslint.org/)

TSlint๊ฐ ๋ฐ๋ก ์์์ง๋ง ESLint์ ํก์๋จ 

### ์ฅ์ 

- ์คํ์ผ ํต์ผ โ ์ฝ๋ฉ ์ปจ๋ฒค์([Coding_conventions](https://en.wikipedia.org/wiki/Coding_conventions)) ์ ์ง ๊ฐ๋ฅ    
- ์ ์ฌ์  ๋ฌธ์  ๋ฐ๊ฒฌ
- ๋ฒ ์คํธ ํ๋ํฐ์ค ์ถ์ฒ โ ์ต์  ํธ๋ ๋๋ฅผ ํ์ตํ๋๋ฐ ํ์ฉ ๊ฐ๋ฅ
  - Ex. `const { value } = data;`  

### ๊ธฐ๋ณธ ์ธํ 

`.vscode` ํด๋ ์์ฑ ํ `settings.json` ํ์ผ ์ถ๊ฐ   

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

* `"source.fixAll.eslint": true` : JS/TS ํ์ผ์ ์ ์ฅํ  ๋๋ง๋ค ESLint๋ฅผ ์คํํ๊ณ  fixํ๋๋ก ์ค์ 
* `"trailing-spaces.trimOnSave": true` : ๋ถํ์ํ ๊ณต๋ฐฑ์ ์ญ์ 

### ์คํ

`npm run lint && npm run check`   
๋ฆฐํธ fix & ํ์์คํฌ๋ฆฝํธ ์ปดํ์ผ ์ฒดํฌ

#### package.json ํ์ผ์ scripts (๊ด๋ จ ๋ถ๋ถ)

```
"scripts": {
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
  },
```
