---
title: "[UML 2.0] 컬레보레이션 다이어그램(Collaboration Diagram)"
date: 2021-01-16T18:48:45+09:00
series:
- 객체 지향 설계
categories:
- UML
tags:
- UML
- OOP
- Java
draft: false
description: >
    UML2.0에서 디자인 패턴을 표현하는 도구
---

- UML 2.0에서 `디자인 패턴`을 표현하는 도구
- 객체들이 특정 상황에서 수행하는 역할의 `상호작용`을 작성

> UML 1.X에서 사용되던 `컬레보레이션 다이어그램`은 2.0에서 `통신 다이어그램(Communication diagram)`으로 변경되었다. 버전에 따라 다이어그램 이름이 상이하니 혼동하지 않도록 주의해야 한다.

작성법
---

- 점선으로 된 타원 기호 사용
- 타원 내부에 협력을 필요로 하는 역할들과 그들 사이의 연결 관계를 표현
- 역할의 클래스를 명시하고자 할 때는 `<role_name>:<class_name>`으로 표현

컬레보레이션
---

![collaboration-diagram](/images/uml/collaboration-diagram/collaboration-diagram.jpg#center)

- 담보 대출 관계를 보여주는 컬레보레이션
    - `대출자`, `대출인`, `담보`라는 역할이 필요
    - 그들 사이의 협력이 요구되므로 이 역할들을 커넥터로 연결 
- 컬레보레이션은 역할들의 상호작용을 추상화한 것
    - 특별한 상황에 적용하면 많은 시스템 개발에 재사용 가능

컬레보레이션 어커런스
---

> 컬레보레이션에 대한 특별한 상황에 대한 적용

`컬레보레이션 어커런스(Collaboration Occurence)`는 구체적인 상황에서의 컬레보레이션 적용을 표현해준다.

![collaboration-occurence](/images/uml/collaboration-diagram/collaboration-occurence.jpg#center)

- `담보 대출` 컬레보레이션을 은행에서 집을 담보로 대출하는 경우에 적용
    - 대출자 = 은행
    - 담보 = 집
    - 대출인 = 사람
- 이 경우 `은행집담보대출`은 `담보대출`의 한 예이며 컬레보레이션 어커런스이다.

참고문헌
---

- [정인성, 채흥석,『JAVA 객체지향 디자인 패턴』, 한빛미디어](http://www.yes24.com/Product/Goods/12501269)
