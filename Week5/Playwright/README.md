# 4. Playwright

## 1. Playwright

[Playwright](https://playwright.dev/)  
[Playwright Configuration](https://playwright.dev/docs/test-configuration)  
[Ashal의 Playwright](https://github.com/ahastudio/til/blob/main/test/playwright.md)

💡 웹 브라우저 기반 E2E 테스트 자동화 도구  
Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원

### Playwright 패키지 설치

```bash
npm i -D @playwright/test eslint-plugin-playwright
```

`playwright.config.ts` 파일


### 테스트 실행

```bash
npx playwright test
```

`.gitignore` 파일에 에러 상황의 스크린샷 등이 담기는 `test-results` 디렉터리 추가

```
/test-results/
```

## 2. E2E(End to End) Test

[What is End-to-End (E2E) Testing?](https://katalon.com/resources-center/blog/end-to-end-e2e-testing)

End To End 테스트    
애플리케이션의 흐름을 처음부터 끝까지 테스트하는 것 

응용 프로그램 흐름이 예상대로 작동하는지 확인하기 위해 처음부터 끝까지 전체 소프트웨어 제품을 테스트하는 방법    
제품의 시스템 종속성을 정의하고 통합된 모든 부분이 예상대로 함께 작동하도록 함  
주요 목적은 실제 사용자 시나리오를 시뮬레이션하고, 
통합 및 데이터 무결성을 위해 테스트 중인 시스템과 해당 구성 요소의 유효성을 검사하여 최종 사용자의 경험에서 테스트하는 것

### 장점 

* 테스트 범위 확장
* 애플리케이션의 정확성을 보장
* 출시 기간 단축
* 비용 절감
* 버그 감지

<br>

## 3. Headless Chrome

[Headless Chrome](https://developer.chrome.com/blog/headless-chrome/)  
[headless browser](https://ko.wikipedia.org/wiki/%ED%97%A4%EB%93%9C%EB%A6%AC%EC%8A%A4_%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80)

### Headless Browser

헤드리스 브라우저(headless browser)는 그래픽 사용자 인터페이스(GUI)가 없는 웹 브라우저  
헤드리스 브라우저는 눈에 보이는 UI 셸이 필요하지 않은 **자동화된 테스트** 및 **서버 환경**을 위한 도구

구글 크롬 버전 59부터 브라우저의 원격 제어를 위한 네이티브 지원이 제공, Mac 및 Linux에서 사용 가능

### 사용 예시 

* 실제 웹 페이지에 대해 테스트를 실행
* 브라우저가 URL을 렌더링하는 방법을 검사

<br>

## 4. Puppeteer

[puppeteer](https://developer.chrome.com/docs/puppeteer/overview/)

크롬 자동화를 위한 Node.js 라이브러리  
DevTools 프로토콜을 통해 헤드리스 Chrome 또는 Chromium을 제어하기 위한 고급 API를 제공하는 노드 라이브러리  
전체 (non-headless) Chrome 또는 Chromium을 사용하도록 구성할 수도 있음


<br>

## 6. CodeceptJS

인간 친화적인 E2E 테스팅 도구로 CodeceptJS

[CodeceptJS](https://codecept.io/)  
[CodeceptJS 3 시작하기](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)  
[CodeceptJS 사용](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-사용)  
