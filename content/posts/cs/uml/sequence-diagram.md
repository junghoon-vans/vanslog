---
title: "[UML 2.0] 순차 다이어그램(Sequence Diagram)"
date: 2021-01-23T16:10:03+09:00
series:
  - UML 설계
categories:
  - UML
tags:
  - UML
  - OOP
  - ☕️ Java
draft: false
summary: >
  객체들의 상호작용을 나타내는 다이어그램
---

객체들의 상호작용을 나타내는 다이어그램 중 하나로 객체들 사이의 메시지 송신과 그들의 순서를 나타낸다.

구성요소
---

### 객체

- 가장 윗부분에 표현
- 왼쪽에서 오른쪽으로 객체들을 나열
- `객체이름 : 클래스이름` 형식으로 표기

### 생명선

- 객체 아래에 이어지는 점선
- 해당 객체가 존재함을 의미

### 활성 구간

- 생명선을 따라서 좁고 긴 사각형
- 객체가 연산을 실행하는 상태
- 실행 시간을 고려해서 적당히 설정

메시지
---

- 형식
    - `[시퀸스 번호][가드]: 반환 값:=메시지 이름([인자 리스트])`
- 화살표로 표시
    - 시작 부분: 송신 객체
    - 끝 부분: 수신 객체
- 가드(Guard)
    - 메시지가 송신되는 데 `만족해야 하는 조건`

### 유형

![message-types](/images/uml/sequence-diagram/message-types.jpg#center)

| 유형 | 의미 |
|:----:|:----:|
| 동기 메시지 | 메시지 실행이 종료될 때까지 다음 작업을 수행할 수 없음 |
| 비동기 메시지 | 메시지 실행이 끝나기를 기다리지 않고 다음 작업 수행 가능 |
| 반환 메시지 | 메시지가 종료되었음을 의미하며 반드시 표기해야 하는 것은 아님 |
| 자체 메시지 | 자신에게 보내는 메시지 |

### 스테레오 타입

- <<create>>
    - 객체를 생성하는 메시지
- <<destroy>>
    - 객체를 소멸시키는 메시지
    - 생명선 끝에 `X`를 넣음

프레임
---

- 모든 다이어그램에 경계, 타입, 이름을 포함한 레이블의 장소를 제공(UML 2.0)
- 다이어그램을 에워싸는 박스로 표시
- 박스 안 왼쪽 모서리에 다이어그램 타입과 이름을 표시
    - sd: 순차 다이어그램
    - uc: 유스케이스 다이어그램
    - act: 액티비티 다이어그램

![sequence-diagram-using-frame](/images/uml/sequence-diagram/sequence-diagram-using-frame.jpg#center)

도서관에서 회원에게 도서를 대여하는 과정을 순차 다이어그램 프레임으로 표시한 것이다. 다만 위 다이어그램에는 대여에 성공한 경우만 나타내고 있다. 만약에 실패한 경우도 나타내고 싶다면 어떻게 해야 할까?

![sequence-diagram-using-alt-keyword](/images/uml/sequence-diagram/sequence-diagram-using-alt-keyword.jpg#center)

이 경우에는 `alt 키워드`를 사용해서 상호작용을 조건에 따라 선택적으로 수행할 수 있게 하면 된다. 

![sequence-diagram-using-loop-keyword](/images/uml/sequence-diagram/sequence-diagram-using-loop-keyword.jpg#center)

회원의 비밀번호를 검증하는 과정을 몇 차례 반복하는 것을 표현하고 싶은 경우 `loop 키워드`를 사용하면 된다.

> 단순히 다른 순차 다이어그램을 참조할 때는 `ref 키워드`를 사용한다.

참고문헌
---

- [정인성, 채흥석,『JAVA 객체지향 디자인 패턴』, 한빛미디어](http://www.yes24.com/Product/Goods/12501269)
