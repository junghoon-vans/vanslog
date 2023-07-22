---
title: "GCP 인스턴스 만들기"
date: 2020-12-02
series:
  - 🫡 군대에서 코딩하기
categories:
  - 개발 환경
tags:
  - 개발 환경
  - GCP
  - 클라우드
draft: false
summary: >
  나만의 평생 무료 인스턴스 만들기
---

서론
---

최근 아니 이미 몇 년간 이어져온 트렌드는 `Cloud`이다. 어디서나 접속 가능한 컴퓨팅 자원 및 서비스를 제공하는 것인데, 이것이 가져오는 편의성은 어마어마하다. 우리는 더 이상 내가 가진 자원에 종속되지 않아도 됨을 의미하고, 어디에 있든 언제든지 간에 클라우드 자원에 접속하여 이전 작업을 이어갈 수 있다.

실제로 클라우드의 수혜를 보는 것은 서비스를 사용하는 사용자보다도 개발자이다. 사용자 입장에서는 클라우드 서비스니 뭐니 해도 레거시한 서버와 차이를 느끼기 힘든 반면, 개발자 입장에서는 클라우드를 도입해서 서버 관리에 이점이 확실하기 때문이다.

개발자에게 클라우드가 이로운 점이 이뿐만이 아니다. 개발서버를 구축하는 것은 개발환경에 있어서의 종속성을 벗어날 수 있게 해준다. 로컬에서 작업한 작업물은 다른 PC에서 이어서 작업하기 위해서 준비해야 할 것들이 사라진다. 물론 git으로 프로젝트를 관리한다고 하는 방법으로 해결할 수 있지 않느냐라는 질문이 있을 수 도 있겠다. 하지만 종속적인 환경들을 다른 PC에서 매번 준비한다는 것은 불편하다.

> 특히나 나와 같이 군대에서 코딩하려는 사람에게는 클라우드 환경 만큼이나 유용한 것은 없을 것이다.

대표적인 클라우드 제공업체로는 `AWS(Amazon Web Service)`, `MS Azure`, `GCP(Google Cloud Platform)`이 있다. AWS는 1년간 매달 750시간 사용 가능한 리눅스 인스턴스를 제공하며, 애저 또한 무료 크레딧을 제공한다. 하지만 오늘 소개할 것은 평생 무료 인스턴스를 제공하는 GCP이다.

GCP가 신규 회원에게 제공하는 혜택은 아래와 같다.

```plaintext
- 평생 무료 이용 가능한 인스턴스
- 3달 동안 이용 가능한 $300 크레딧
```

인스턴스 생성
---

인스턴스 생성에 앞서 [구글 클라우드 플랫폼(GCP)](https://cloud.google.com/)에 가입하자. 가입하는 과정은 그리 어렵지 않으니 따로 설명하진 않겠다. 회원가입이 되었다면 우선 아무런 이름이든 상관없으니 프로젝트를 생성한다. 이를 위해서 [콘솔](https://console.cloud.google.com/)로 접속해서 진행한다.

![create-gcp-instance1](/images/development-environment/create-gcp-instance/create-gcp-instance1.png#center)

앞에서의 과정이 모두 끝났다면 인스턴스를 생성해준다. 생성한 프로젝트에서 햄버거 메뉴를 눌러 `Compute Engine-VM 인스턴스`를 클릭해서 들어가주고, 위 사진에 보이는 플러스 버튼을 눌러 인스턴스 생성을 시작하자.

![create-gcp-instance2](/images/development-environment/create-gcp-instance/create-gcp-instance2.png#center)

여기서 이름은 원하는 대로 지어주면 되고, 중요한 부분은 리전과 머신 구성 부분이다. `평생 무료 인스턴스`를 만들기 위해서는 조건이 있는데, 우선 region을 `us-east1-b`를 사용해야 하고 머신 유형은 `f1-micro`를 사용해야 한다.

> 만약 제공되는 $300 크레딧 사용이 목적이라면 다른 리전과 머신 유형으로 구성하는 것을 추천한다. 평생 무료 인스턴스의 성능이 매우 낮기 때문이다.

![create-gcp-instance3](/images/development-environment/create-gcp-instance/create-gcp-instance3.png#center)

이제 설정할 것은 부팅 디스크와 방화벽 설정뿐이다. 웹 서버로 이용할 서버를 구성하고 싶다면 HTTP, HTTPS를 허용을 해주어야만 한다.

![create-gcp-instance4](/images/development-environment/create-gcp-instance/create-gcp-instance4.png#center)

부팅 디스크 설정의 변경 버튼을 눌러서 다른 OS를 선택할 수 있는데 나는 우분투를 선호해서 `Ububtu 20.04 LTS`를 선택했다. 참고로 디스크는 30GB까지가 무료로 제공되는 용량이다.

이렇게까지 설정해서 만들기를 누르면 인스턴스가 생성되며, GCP에서 지원하는 web SSH 연결을 이용해서 쉽게 접속해서 이용할 수가 있다.