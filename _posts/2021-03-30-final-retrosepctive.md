---
title:  "베이스캠프 최종 회고"
tags:
  - NHN
  - 루키
  - 베이스캠프
  - 기술교육
  - 회고
categories:
  - NHN루키



---

## 베이스캠프 종료

사전과제 기간을 포함하여 10주 간의 베이스캠프가 종료되었다.

베이스캠프 기간 동안 배운 내용은 아래와 같다.

### 1주차 사전과제 

 ```코드로 배우는 스프링 부트 웹 프로젝트 ``` 교재를 따라서 스프링 부트와 JPA를 활용한 웹 애플리케이션을 구현해 보았다.

### 2주차 기획하기

본격적인 베이스캠프의 시작이었다. **예매 시스템**이라는 큰 주제 아래에서 자유롭게 웹 서비스를 기획해 보았다.

### 3주차 개발 준비

Git/Github 사용법, vim 사용법, bash 튜토리얼, 스프링 부트 애플리케이션 구동을 위한 리눅스 서버 설정 등을 배우며 본격적인 개발을 준비하였다.

### 4~5주차 개발하기

2주차에 기획하였던 웹 서비스를 구현하였다. 설 연휴가 포함되어 있었고, 내가 소속된 판다TF의 경우 설 연휴에도 개발을 진행하였다. 그럼에도 불구하고 개발 막바지에는 하루 4시간 정도의 수면 시간을 유지하며 개발에 몰두해야 했다.

### 6주차 클린코드

짧은 시간 내에 바쁘고 정신 없이 개발했던 예매 서비스를 리팩토링해야 했다.  소나큐브의 정적 분석 결과에 따라 버그가 우려되는 코드 및 가독성이 떨어지는 코드 등을 수정했고, 단위/통합 테스트 추가를 통해 테스트 커버리지를 향상시켰다. 

### 7주차 웹 서버 스케일 아웃

트래픽이 증가하는 상황을 가정하여, 기존의 웹 서버 1개에서 구동되던 예매 서비스를 웹 서버 2개로 확장하여 구동해 보았다. 이를 위해 회원의 세션 정보라든지 업로드된 파일에 대한 접근 및 생성이 양쪽 서버 모두에서 정상적으로 이루어지도록 구현해야 했다.

이를 위해 공유 세션의 경우 Redis와 RDBMS를 활용하는 2가지의 방식으로 구현해 보았고, 실제 서버에서는 Redis를 활용하는 방식을 사용하였다. 또한 영화 포스터의 경우 저장 위치를 기존의 로컬 디스크로부터 NHN 클라우드의 오브젝트 스토리지로 옮겨 양쪽 서버 모두에서 접근할 수 있도록 구현하였다.

### 8주차 DB 샤딩 & 서비스 통합

DB의 트래픽 증가 및 데이터 누적을 가정하여, 기존의 DB 서버 1개에 저장되던 회원 관리 테이블을 DB 서버 2개로 확장하였다. 회원 ID의 해시값을 %(modulo) 연산한 값을 기준으로 서로 다른 DB로 접근하도록 구현하였다.

또한 DB 확장의 맥락에서 활용될 수 있는 셀 아키텍처(Cell-based architecture)를 체험해 보는 의미에서 여러 TF 간 로그인을 통합해 보았다. 다른 TF의 웹 서비스로부터 우리 TF의 로그인 API로 접근할 경우, 정상 회원 여부를 인증한 뒤 우리 TF의 도메인으로 리다이렉트를 받아야 했다. 이를 위해 인증과 리다이렉트를 통한 접근 사이에 상태 정보를 저장하기 위한 토큰 관리 기능을 구현해야 했다. 더불어 TF 간 로그인 관련 API의 스펙을 조율하는 과정 또한 필요했다. 약간의 혼선은 있었지만 다행스럽게도 빠르게 해결되었고, 결과적으로는 무사히 구현되었다.

### 9주차 안정적인 운영

안정적인 운영을 위해 여러 방식의 서버 모니터링을 경험해 보았다. Watchdog과 nSight를 통해 서버의 이상 작동을 확인하고, Log&Crash를 통해 원격으로 중요 로그를 확인할 수 있었다. 그리고 필요한 경우 메신저/메일 등을 통한 알림이 이루어지도록 설정할 수 있었다. 또한 서버 내부 측면에서는, 쉘 스크립트 작성을 통해 접속 로그를 확인할 수 있도록 하였고, 크론탭을 통해 웹 애플리케이션 프로세스 중단 시 자동 재구동이 이루어지도록 하였다.

### 10주차 출시

QA 결과에 따라 수정된 코드를 프리징하고 리얼 서버에 배포하였다. 메일을 통해 전사에 판다TF의 서비스 도메인이 공유되는 ~~부끄러운~~ 경험 또한 할 수 있었다. 

## 기억에 남는 부분

### 웹개발 대면기

입사 전까지만 해도 네트워크는 거의 글로만 배웠고, http 통신을 처리해 본 경험은 파이썬의 requests 라이브러리를 통해 rest api를 호출해 보는 정도가 전부였다.

10주의 시간 동안 웹 애플리케이션을 개발하고 확장하고 운영하는 과정 속에서 여러 에러들과 부딛혔고, 막힐 때에는 운영진과 베이스캠프의 동료들로부터 많은 것들을 배웠다. 어쨌든 우여곡절 끝에 웹 애플리케이션을 판다TF와 함께 만들었고 정상적으로 작동되는 것을 확인했다.

나 같은 웹알못에게는 그 정도면 충분한, 아니 어쩌면 기대 이상의 발전이라고 생각한다.

### 스프링 부트 대면기

스프링 부트를 처음으로 접하고 사용해 보았다. 스프링 부트의 경우에는 전사적으로 활용도가 높은 상황이다. DB와 관련한 코드에는 JPA를 활용하기도 했지만, 아직은 MyBatis를 활용하는 부서가 더 많은 것으로 보인다. 따라서 스프링 부트를 짧은 시간이나마 접해본 것이 가장 큰 수확이라 생각한다.

스프링 부트의 작동 방식에 대해 심층적으로 이해하지는 못하였지만, 어설프게나마 그 기능들을 작동시키며 그 사용법을 익힐 수 있었다. 대략적인 작동 흐름은 아래와 같았다.

- 애플리케이션으로 http 요청이 전달된다.
- 필터 또는 인터셉터는 요청을 가로채어 전처리를 하거나 필요한 경우 접근을 차단한다.
- 컨트롤러는 요청에 대해서 적절한 서비스 계층 메소드를 호출한다. 
- 서비스 계층에서는 레포지토리 계층 메소드 호출을 통해 DB와 통신하고 그 결과를 컨트롤러에 반환한다.
- 컨트롤러에서는 서비스 계층의 처리 결과에 따라 적절하게 가공된 http 응답을 반환한다.
- 필터 또는 인터셉터는 응답을 가로채어 후처리를 하거나 필요한 경우 접근을 차단한다.

그리고 위와 같은 애플리케이션 내에서의 계층 간 요청/응답이 이루어질 때마다 상시적으로

- Aspect를 통한 횡단 관심사의 처리가 이루어진다.
  - ex) 컨트롤러 어드바이스를 통한 예외 처리 등

또한 단순히 프레임워크의 활용법을 익히는 것 이상으로 개념 및 설계 구조에 대한 이해도 심화할 수 있었다. 스프링 부트는 MVC 패턴과 의존성 역전이라는 컨셉에 기반하고 있고, 따라서 스프링 부트에 익숙해지는 과정 속에서 그러한 개념들에 대한 이해를 심화할 수 있었다. 더불어 후반부의 리팩토링 과정에서는 여러 메소드에 걸쳐 반복적으로 나타나는 작업을 AspectJ를 통해 처리해 보았고, 이를 통해 AOP에 대해서도 얕게나마 이해해 볼 수 있었다. 

### Best Practice를 향하여

나의 경우에는 개발자로서의 커리어를 선택한 시기가 다른 사람들에 비해 매우 늦었고, 이 때문에 취업 전까지만 해도 지식과 경험의 범위를 빠르게 확대하는 것이 주된 목표였다. 따라서 여러 가지 언어, 여러 가지 라이브러리들을 빠르게 훑어보고, 그러한 도구들을 활용해 일단 돌아가는 코드를 작성해 보는 것이 사실상 개발 경험의 전부였다. 지식은 늘어났지만, 개발 스킬은 깊어지지 않았다.

하지만 이번 교육에서는 ```스프링 부트 + JPA + 바닐라 자바스크립트``` 라는 큰 틀 안에서 10주 간 개발을 꾸준하게 진행하였다. 또한 코드 리뷰, 정적 분석 등을 통해 스스로 작성한 코드를 돌아보는 시간을 가질 수 있었다.

그러다 보니 자연스럽게 나쁜 코드의 유형을 인지하고 최선의 구현, 최선의 설계에 대해 고민해 볼 수 있었다. 그러한 고민의 결과, 적절한 디자인 패턴을 적용하여 장황하고 반복적인 코드를 리팩토링해 볼 수 있었다. 또한 예외 처리 등에 대해서도 스프링 부트의 다양한 예외 처리 기능들을 적용하며 최선의 방식을 찾기 위해 노력하였다.

물론 지식과 시간의 부족으로 인해 최선의 구현에 도달하지는 못했지만, 최선의 구현에 대한 관심을 갖게 된 것만으로도 큰 성취라고 생각한다. 이러한 관심을 지식과 실력으로 발전시켜 나가는 것은 앞으로 내가 해결해 나가야 할 과제이다.

## 인사 발령

나는 공식적으로는 2021년 4월 1일부터 클라우드서비스개발팀으로 발령받아 근무하게 되었다. 

2지망으로 지원한 부서이기는 하지만, 지금 생각해 보면 1지망으로 지원했던 컴퓨팅인프라개발팀보다 오히려 나에게는 좋은 배치가 아닐까 싶다. 웹 개발은 여러 곳에서 요구되는 범용적 기술이기에 개발자로서의 커리어를 시작하기에는 좋은 지점이라 생각한다. 또한 베이스캠프를 통해 갖게 된 디자인 패턴 등에 대한 관심을 더욱 발전시키고 코드에도 반영해 나갈 수 있으리라는 기대가 크다.

또한 노력하기에 따라서 클라우드 아키텍처에 대한 이해를 심화해 나갈 수 있는 점 역시 매력적인 포인트라고 생각한다. 나에게 있어 개발을 학습하는 과정에서 가장 매력적이었던 경험들 중 하나는 C언어를 통한 로우 레벨 개발 경험이다. 클라우드 관련 조직의 특성상, 내가 노력하기에 따라 로우레벨의 지식을 습득하고 활용할 수 있는 기회가 있으리라고 기대된다.

팀에서 사용 중인 vue.js 경험이 없어 약간의 학습 시간이 필요하겠지만, 다른 프론트엔드 프레임워크에 비하면 진입장벽이 낮다고 하니 학습에 있어 큰 무리는 없을 듯하다.

## 마치며

사실 이제부터가 본격적인 시작이다. 

교육을 시작할 당시에도 다른 동기들에 비하면 나의 실력이 부족했고, 지금도 그 사실은 변함이 없다. 하지만 부족함에 대한 조바심은 내려놓고, 꾸준함으로 그 격차를 메워 나가고자 한다.

베이스캠프 때와는 조금 템포가 다를지라도, 어제의 진정성을 잊지 않고 앞으로의 직장생활을 이어나가야겠다.