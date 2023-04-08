# 한 주를 마치며

## 회고

프론트엔드 테스트에 대해 배운 한 주였다. 다른 주차보다 훨씬 어렵고 부담스러운 내용이었다. 
한 번도 테스트 코드를 직접 작성해 본 적이 없고, 테스트를 하기 위해 필요한 **테스트적 사고 방식**이 없었기 때문이다.
특히 TDD는 단지 테스트 코드를 작성하기만 한다고 해서 TDD가 되는 것이 아니었다. 
중복 제거를 통해 패턴을 찾아내고 계속 리팩토링을 진행해야 하는 과정이 어렵게 느껴졌다. 

1주차에서 **Mocking**을 처음 알게되어 학습이 더 필요하다고 적어두었는데 이번 기회에 모킹이란 무엇인지, 왜 사용하는지 
이해하게 되었다. MSW를 공부하며 서비스 워커에 대해서도 학습을 했다. 
이벤트 루프와 관련해서 웹 워커가 무슨 역할을 하는지 알아본 적이 있었는데 그때 본 서비스 워커를 다시 만나게 되었다.
자바스크립트와 프론트엔드를 학습하다 보면 별개의 개념 같아도 **유기적으로 엮여있다**는 것을 느끼게 된다. 

과제를 하며 CodeceptJS를 이용한 E2E 테스트를 몇 번 이용해 봤었다. 
그래서 **Playwright**과 **CodeceptJS**는 이번 주차 학습 내용 중 익숙하고 이해가 잘 되었다. 
혹은 사용자로서 접근하는 관점이기 때문에 개발단에서 테스트가 진행되는 단위 테스트보다 익숙할 수 밖에 없는 것일지도.
다양한 테스트를 실습해보며 테스트가 어떤 것인지 대략 훑어보았는데, 직접 테스트 코드를 작성하고 자유자재로 이용하려면 
시간과 노력이 많이 필요할 것 같다.

<br>

## 💡 새로 알게된 내용

- TDD
  - TDD가 되기 위한 조건(TDD Cycle) 
  - 중복 제거 
  - Jest 사용 방법 
- React Testing Library
  - given - when - then 패턴
  - Test fixture
- MSW
  - MSW의 개념
  - 폴리필
- Playwright
  - E2E 테스트 도구의 종류 
  - Headless Browser


## 🔎 이해가 깊어진 내용

- React Testing Library
  - Mocking
- MSW
  - Service Worker
  - 폴리필
- Playwright
  - E2E(End to End) Test
  - Playwright 사용 방법 
  - CodeceptJS
  

## 🤔 학습이 더 필요한 내용

- TDD
  - TDD식 사고방식 
  - Describe - Context - It 패턴
  - 중복 제거
  - SWC
  - 단위 테스트
- React Testing Library
  - BDD 스타일 테스트 코드 작성하기
- MSW
  - MSW를 이용해 REST API 모킹하기
- Playwright
  - Playwright 이용해 E2E 테스트 코드 작성하기 
