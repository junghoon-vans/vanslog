---
title: "[Spring] 왜 스프링인가"
date: 2022-07-21T22:47:38+09:00
draft: false
series: 
  - 🍃 Spring 스터디
categories: 🍃 Spring
tags:
  - ☕️ Java
  - 🍃 Spring
  - J2EE
  - EJB
---

스프링은 엔터프라이즈 환경을 위한 `애플리케이션 프레임워크`이며, 오늘날 자바 기반 웹 애플리케이션 개발의 표준입니다. 스프링을 완전히 이해하기 위해서는 이것의 `등장 배경`을 알고 있을 필요가 있습니다. 

왜냐하면 스프링은 엔터프라이즈 애플리케이션 개발을 위한 `J2EE(Java 2 Platform, Enterprise Edition)`의 문제점을 해결한 결과물이기 때문입니다. 스프링의 특징이나 장점들이 어떠한 배경 속에서 등장했는 지를 이해하면 더욱 스프링에 대한 이해가 높아질 수 있을 것입니다.

# J2EE

`J2EE(Java 2 Platform, Enterprise Edition)`는 엔터프라이즈 애플리케이션 개발을 위한 스펙의 집합입니다. 일반적으로 사용하는 자바인 `J2SE(Java 2 Platform, Standard Edition)`를 확장하여 만들어 졌습니다.

> 💡 `J2EE`와 `J2SE`는 버전에 따라 명칭이 다르게 불립니다. 버전 5가 릴리즈된 시기에 `Java EE`와 `Java SE`로 명칭이 변경되었습니다. `Java EE`의 경우 오라클이 운영권을 이클립스 재단으로 이관하면서 `Jakarta EE`로 한 차례 더 변경되었습니다.

## 스펙

- J2SE APIs
- Servlet
- JSP
- EJB
- ...

J2EE에는 기본적으로 앞서 말한 것과 같이 J2SE를 기반으로 하고 있으며, 엔터프라이즈 애플리케이션 개발에 필요한 요소들을 추가한 것입니다. 대표적으로 `Servlet`과 `JSP`, `EJB`가 있습니다. 한 번쯤 자바 기반의 웹 프로그래밍을 접해봤다면 들어는 보셨을 수도 있을 것입니다.

## Application Server

`J2EE`는 반드시 이것의 스펙을 만족하는 `J2EE 애플리케이션 서버(J2EE Application Server)`를 필요로 합니다. 이것을 통상적으로 `WAS(Web Application Server)`라고 했습니다. 대표적인 WAS로는 `JEUS`, `Weblogic`, `WebSphere` 등이 있습니다.

> 💡 최근에는 자바뿐 아니라 `Node.js`나 `Flask`와 같이 다른 언어로도 백엔드 서버를 구축할 수 있으므로 더 이상 `WAS == J2EE Applcation Server`라는 말할 수는 없습니다.

## 문제점

결정적으로 J2EE에서 Spring으로 패러다임이 변화하게 된 것에는 `세 가지` 이유가 있습니다.

### 운영 비용

우선 J2EE는 운영환경의 요구사항이 매우 높았습니다. J2EE의 스펙을 모두 충족하는 풀스택 서버를 구축하기 위해서는 매우 `고가의 서버가 요구`되었고 운영 비용이 천문학적이었습니다.

### 플랫폼 종속성

J2EE의 목적은 자바의 플랫폼 독립적인 특성을 활용하여, `환경에 종속적이지 않은 미들웨어`를 제공하는 것이었습니다. 하지만 이러한 목적과 달리 실제 J2EE 애플리케이션은 `플랫폼 종속적`이었습니다.

근본적으로 J2EE에 정의되지 않은 기능들을 많았던 것이 이유였습니다. 이는 결국 WAS의 제조사별로 구현방식이 달라지게 되었으며, 운영환경의 종속성을 탈피하려는 시도는 실패했습니다.

### EJB라는 겨울

`EJB(Enterprise Java Bean)`은 J2EE 환경에서 비즈니스 로직을 처리하는 분산 컴포넌트 기술입니다. 이것의 취지는 좋았으나 사용하기에 너무나도 복잡성이 높았습니다. `Spring Framework`의 개발자인 `Rod Johnson`은 `J2EE Development without EJB`라는 책에서 아래와 같이 말했습니다.

*"EJB와 같은 실패를 만들지 않기 위해서 가장 먼저 생각할 것은 기본 핵심가치(core values)인데 그것은 6가지로 구성된다."*

```plaintext
- Simplicity
  - Scale down이 되지 않는 EJB같은 복잡한 아키텍처가 아니라
  - 필요에 따라서 아키텍처 리팩토링에 의해서 scale up할 수 있는 심플한 아키텍처를 추구하는 것이 바람직하다.
- Productivity
  - 실무에서 가장 중요한 것은 결국 생산성이고 그로 인한 비용절감&시간단축이 아니겠는가.
- OO
  - Java는 그 자체로 훌륭한 OO언어이다. 왜 특정기술(EJB)등이 OO에 선행하여 좋은 OO디자인을 방해하는가?
- Primacy of Requirement
  - 핵심요구사항에 집중하기.
- Empirical Process
  - 스스로 확인하고 테스트하고 검증된 기술과 구조를 사용해야 한다.
  - IT계는 희한하게도 감정적인 유행도 많고 검증되지 않은 추측과 유언비어가 난무한다.
- Testability
    - TDD니 TFD(Test First Development)등이 유행하고 있다.
    - 좀 지나치게 유행하는게 아닌가 싶기도 한데 그래도 그 중요성을 무시할 수 없다.
```

# Spring

스프링은 Spring Framework의 개발자 Rod Johnson이 `J2EE Design and Development`라는 책을 통해 기존 J2EE의 문제점을 지적한 것이 시작입니다. EJB 없이도 충분히 고품질의 확장 가능한 애플리케이션을 개발할 수 있음을 30,000줄짜리 예제 코드를 통해 증명했습니다.

책 출간된 이후 Juergen Hoeller와 Yann Caroff는 `오픈 소스 프로젝트`를 제안하였고, 이것이 바로 오늘날의 스프링 프레임워크가 되었습니다. 스프링이라는 이름은 Yann Caroff가 제안한 것으로 `J2EE라는 겨울을 거친 새로운 시작`이라는 의미입니다.

*"the fact that Spring represented a fresh start after the “winter” of traditional J2EE"*

## 특징

### 내장 WAS

스프링은 J2EE 스펙을 모두 구현한 WAS가 필요하지 않습니다. 다시 말해 `서블릿 컨테이너` 기술만 구현된 WAS만으로도 충분히 동작합니다. 기본적으로 스프링은 `톰캣(Tomcat)`이나 `제티(Jetty)`를 내장하고 있으므로 별도의 WAS를 설치할 필요가 없습니다.

### POJO

`POJO(Plain Old Java Object)`는 일반적인 자바 객체를 말합니다. EJB와 같은 프레임워크에 종속되는 오브젝트를 사용하는 것에 대한 반발로 나온 단어로 `마틴 파울러`가 처음 만들었습니다.

POJO는 프레임워크 종속성 문제를 해결하기 위한 스프링의 핵심입니다. POJO 기반 프레임워크이므로 스프링을 사용하면 객체지향적이며 간결한 코드를 짤 수 있고 테스트를 쉽게 할 수 있습니다.

## J2EE와의 관계

스프링을 공부하면서 가장 헷갈렸던 부분이 바로 J2EE와의 관계입니다. 검색 결과에 따라 스프링이 `J2EE를 대체`하는 기술이라고 설명하기도 하지만, `J2EE 개발을 도와주는` 프레임워크라고 소개되기도 합니다. 도저히 양립될 수 없는 두 가지 설명이 혼란을 가중시킵니다. 과연 어떤 것이 맞는 설명일까요?

결론부터 말씀드리자면 `스프링은 J2EE를 대체하는 기술`이 맞습니다. 하지만 그렇다고 해서 J2EE와 전혀 관계가 없다고 하기에도 어렵습니다. 그 이유는 스프링은 `J2EE 기술의 일부를 채용`해서 만들어졌기 때문입니다. 대표적으로 `서블릿`은 여전히 스프링에서 통신하는 부분을 처리하기 위해 사용됩니다. 스프링이 J2EE 스펙을 전부 만족하는 것은 아니지만 `일부 스펙을 만족`하고 있다고는 할 수 있을 것 같습니다.

제 개인적인 생각으로 이러한 오해가 시작된 것은 스프링을 개발한 `Rod Johnson`의 저서 `J2EE Development without EJB` 제목이 문제라고 생각합니다. `EJB 없는 J2EE 개발`이라는 말은 꼭 스프링이 J2EE 개발을 도와주는 것처럼 느껴집니다. 하지만 실제로는 `EJB를 배제하고 J2EE 스펙의 일부만을 상속`한 새로운 프레임워크의 등장을 말하고자 했던 것으로 보여집니다.

# 마치며

스프링을 사용하면서 궁금했던 것들을 정리하다 보니 스프링이 등장하게 된 배경을 정리하게 되었습니다. 이를 통해 얼핏 알고 있던 스프링 이전의 기술들에 대해서 공부하는 시간을 가질 수 있었고, 스프링을 사용하는 이유와 장점에 대해 더 잘 알 수 있었습니다.

여기서 언급하지 못한 내용들이 아직 많이 남아 있습니다. 스프링의 특징 중에 내장 WAS와 POJO만을 언급했는데, 이는 `J2EE의 문제점과 대립되는 특징에 집중`하기 위함이었습니다. 추후 포스팅을 통해 스프링의 다른 특징들과 POJO에 대해 자세한 내용으로 정리하겠습니다.

# 참고 자료

- [Jakarta EE - Wikipedia](https://en.wikipedia.org/wiki/Jakarta_EE)
- [자바EE의 역사 및 스프링과의 관계 - okky](https://okky.kr/article/415474)
- [Spring Boot을 언제 써야 할까? (2) 외장 WAS가 반드시 필요한가? - zepinos님](https://zepinos.tistory.com/86)
- [왜 EJB 없는 J2EE 개발에 대해 이야기하는가? - 토비님](https://cafe.naver.com/sylee999/85)
- [Spring Framework: The Origins of a Project and a Name - Spring Blog](https://spring.io/blog/2006/11/09/spring-framework-the-origins-of-a-project-and-a-name)