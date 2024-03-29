---
title: "[UML 2.0] UML이란?"
date: 2021-01-03T11:22:49+09:00
series:
  - UML 설계
categories:
  - UML
tags:
  - UML
  - OOP
  - 모델링
draft: false
summary: >
  객체 지향 애플리케이션을 모델링하기 위한 언어, UML
---

개념
---

요구 분석, 시스템 설계, 시스템 구현 등의 `시스템 개발 과정`에서 개발자 사이의 의사 소통이 원활하게 이루어지도록 표준화한 `통합 모델링 언어(United Modeling Language)`

다이어그램으로 프로그램을 설명하는 UML이 처음이 아니다. 과거에는 객체 지향 모델링을 위해 `OMT`를 사용하였는데 이것이 UML의 원형이 되었다.

다이어그램
---

### 구조 다이어그램

| 다이어그램 유형 | 목적 |
|----------------|------|
| 클래스(Class) | 시스템을 구성하는 클래스들 사이의 관계 표현 |
| 객체(Object) | 객체 정보를 보여줌 |
| 복합체 구조(Composite Structure) | 복합 구조의 클래스와 컴포넌트 내부 구조 표현 |
| 배치(Deployment) | S/W, H/W, N/W를 포함한 실행 시스템의 물리 구조 표현 |
| 컴포넌트(Component) | 컴포넌트 구조 사이의 관계 표현 |
| 패키지(Package) | 클래스나 유스 케이스 등을 포함한 여러 모델 요소들을 그룹화해 패키지를 구성하고 패키지들 사이의 관계를 표현 |

### 행위 다이어그램

| 다이어그램 유형 | 목적 |
|----------------|------|
| 활동(Activity) | 업무 처리 과정이나 연산이 수행되는 과정 표현 |
| 상태 머신(State Machine) | 객체의 생명주기 표현 |
| 유스 케이스(Use Case) | 사용자 관점에서 시스템 행위를 표현 |
| 상호작용(Interaction) | 목적에 따라 4가지로 분류됨 |

### 상호작용 다이어그램

| 다이어그램 유형 | 목적 |
|----------------|------|
| 순차(Sequence) | 시간 흐름에 따른 객체 사이의 상호작용 표현 |
| 상호작용 개요(Interaction overview) | 여러 상호작용 다이어그램 사이의 제어 흐름을 표현 |
| 통신(Communication) | 객체 사이의 관계를 중심으로 상호작용 표현 |
| 타이밍(Timing) | 객체 상태 변화와 시간 제약을 명시적으로 표현 |

`UML 2.0`에서는 시스템 구조와 동작을 표현하는 13개 다이어그램을 제공한다. 이렇게 많은 종류가 존재하는 이유는 `다양한 관점`으로 시스템을 모델링하기 위함이다.

특징
---

- 가시화 언어
  - 모델링 결과를 가시적으로 나타냄 
- 명세화 언어
  - 정확하고 완전하게 모델링 하는 것
- 구축 언어
  - 시스템을 구축할 수 있게 함
- 문서화 언어
  - 시스템에 대한 통제, 평가, 의사소통의 역할

도구
---

- [draw.io](http://www.draw.io)
  - 무료 웹 기반 UML 드로잉 애플리케이션
- starUML
  - 무료 데스크톱 기반 UML 드로잉 애플리케이션

참고문헌
---

- [정인성, 채흥석,『JAVA 객체지향 디자인 패턴』, 한빛미디어](http://www.yes24.com/Product/Goods/12501269)
