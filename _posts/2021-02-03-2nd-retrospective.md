---
title:  "베이스캠프 2주차 회고"
tags:
  - NHN
  - 루키
  - 베이스캠프
  - 기술교육
  - 회고
categories:
  - NHN루키
---

## 본격적인 팀 프로젝트의 시작

베이스캠프 2주차부터 베이스캠프 기간 종료 시점에 이르기까지는 4인으로 구성된 팀(A.K.A **"TF"**) 단위로 함께 웹 서비스를 만들어 나가는 것이 프로젝트로 주어졌다. 베이스캠프 1주차에서도 서로 모르는 부분에 대해서 토론도 하고 질문도 하면서 많은 도움을 얻었지만, 2주차부터는 보다 본격적인 **소통**과 **협업**이 필요하게 되었다.

## 2주차 - 서비스 기획

### 월요일

월요일 오전에는 앞으로 수행해야 할 프로젝트의 내용에 관해 전달받았다. **예매 서비스**라는 주제와 최소한의 기본적 구현 필요사항이 주어졌고, 그 외의 세부적 사항 및 추가 기능은 각 TF에서 자유롭게 선택할 수 있었다.

2주차의 과제는 서비스의 기획이었다. **주의사항**은 이번 주차 동안은 개발에 **절대로** 손을 대지 말고 기획자의 입장에서 서비스를 기획하라는 것이었다.

더불어 앞으로 과제 수행과 관련해서 자문을 얻을 수 있도록 현업에서 근무 중인 개발자 한 분이 **멘토**로 배정되었다. 멘토 분과 간단한 자기소개 및 베이스캠프 1주차 회고를 공유한 뒤 서비스 기획에 돌입하였다.

오후에는 잠시 브레인스토밍의 시간을 가진 뒤 기획서의 **초안**을 마련하기 위한 회의를 진행하였다. 영화, 공연, 스포츠, 교통수단 등 다양한 대상의 예매 서비스가 개발의 대상으로 제안되었고, 최종적으로는 TF 구성원들 모두 사용 경험이 많았던 영화 예매 서비스를 개발하기로 최종 결정하였다. 그리고 내가 소속된 TF의 이름이 **판다TF**였던 만큼 영화 예매 서비스의 이름은 **무비다판다**로 정하게 되었다.

<center><img src="https://user-images.githubusercontent.com/67428295/106252660-d6c86200-6259-11eb-9c51-920c384d2fe9.png" width="250"></center>

뒤 이어 기능에 관한 기획을 진행하였다. 기본 기능을 위주로 기획을 진행하였음에도 불구하고, 화면의 이동 흐름이라든지 사용자의 선택에 따른 화면상의 반응 등 생각해야 할 부분이 많았다. 이 때문에 시간이 금새 흘러갔다. 그럼에도 불구하고 다행히 퇴근 전에 대략적인 초안을 마련할 수 있었다.

### 화요일

화요일 오전부터는 기본적인 UI 설계를 시작하였다. 툴은 [Balsamiq](https://balsamiq.com/wireframes/)을 선택하였는데, 기능이 단순하여 툴 사용을 위해 별다른 학습이 필요 없었고 클라우드를 통한 공동 작업이 가능하여  작업의 빠른 진전이 가능하였다. 화면을 직접 눈으로 볼 수 있어, 텍스트로만 기획을 진행하던 때보다 높은 흥미와 집중도를 바탕으로 작업을 진행할 수 있었다.

화면을 구성하고, 화면 간의 이동을 시뮬레이션하는 과정에서 기획서 상 구체화가 필요한 부분 또는 오류가 다수 발견되었다. 와이어프레임의 제작 및 기획서의 상세화・보완을 동시에 진행하다 보니 어느새 하루가 지나 있었다.

### 수요일

수요일에는 화요일에 이어 와이어프레임의 제작 및 기획서 보완을 진행하였다. 오후에는 멘토 분과의 회의를 통해 기획의 방향에 대한 피드백을 얻을 수 있었다. 핵심은 "'무비다판다'라는 서비스만의 특징을 추가해 보는 것이 어떻겠냐"는 것이었다. 사실 개발 기간의 압박으로 인해 필수적인 사항을 제외한 추가 기능을 거의 넣지 않았는데, 덕분에 조금 더 폭넓은 상상을 해 볼 수 있었다. 개발 일정에 여유가 있을 경우, 지도에서 영화관 찾기/멤버쉽/스낵 구매/리뷰 등의 추가 기능을 구현하는 방향으로 의견이 모아졌다.

### 목요일

목요일 오전에는 '기민한 소프트웨어 개발을 위한 요구사항 작성과 추정'이라는 주제의 강의가 진행되었다. 강의 막바지에는 TF 구성원들과 함께 사용자의 요구사항을 정리하였고, 요구사항별 구현 기간 추정을 위한 [**'플래닝 포커'**](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9E%98%EB%8B%9D_%ED%8F%AC%EC%BB%A4)를 경험해 보았다. 구성원별로 예상 구현 기간의 차이가 크게 나타났는데, 각자가 가진 개발 경험과 관점에 따라 개발 과정상의 난점 또는 해결책을 서로 다르게 추측하고 있었기 때문이었다. 더욱이 아직은 개발 경험이 적은 루키들인지라 그 예상치의 편차가 클 수밖에 없었다.

오후에는 최종적으로 기획안과 와이어프레임을 정리하였고, 3주차 월요일에 있을 기획안 발표자료의 초안을 준비하였다.

### 금요일

오전에는 기획안 발표자료를 마무리하였고, 오후에는 멘토 분과 함께 발표의 리허설을 진행하였다. 기본 기능과 추가 기능에 대해서 우선순위를 명확하게 정할 필요가 있다는 멘토님의 피드백이 있었고, 마지막으로 2주차에 대한 회고를 함께 나누며 한 주를 마무리하였다.

## 2주차를 정리하며

나의 경우 아무래도 관심사가 프론트엔드에 있지 않다 보니 사용자 경험에 대해서는 많은 관심을 가지지 않아 온 것이 사실이다. 사용자의 요구사항을 중심으로 기획을 이어가다 보니, 서비스의 목적이라든지 전반적인 소프트웨어 제작의 흐름에 대해 더 깊은 이해를 가질 수 있는 시간었다.

또한 명확하고 쉬운 용어 사용의 중요성에 대해서도 느낄 수 있었다. 이를테면 영화 예매 시스템에서 활용하는 '예매율'과 같은 용어는 일상적으로 사용하지만, 그 구체적인 의미나 산정 방식에 대해서는 오해가 발생할 수 있었다. 현업에서 그러한 부분들이 쌓이다 보면 개발 스펙의 미충족이라든지 협업 과정 상에서의 갈등이 발생할 수 있으리라고 생각한다. 앞으로도 특히나 협업 대상(기획자-디자이너-개발자 등) 간의 명료한 소통을 위해 적확한 용어 선택을 위해 노력을 기울일 예정이다.

3주차부터는 본격적인 기술교육이 시작된다. 최대한 많은 지식을 체득하기 위해 노력해야겠다. 화이팅!
