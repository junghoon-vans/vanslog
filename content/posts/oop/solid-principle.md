---
title: "[OOP] 객체지향 설계 원칙: SOLID 원칙"
date: 2021-01-10T19:53:10+09:00
series:
- 객체지향 개념
categories:
- OOP
tags:
- OOP
- JAVA
draft: false
description: >
    객제지향 설계 원칙
---

단일 책임 원칙(SRP)
---

> `단일 책임 원칙(Single Responsiblity Principle, SRP)`: `단 하나의 책임`만 가져야 한다는 원칙

### 책임의 의미

SRP에서 말하는 책임의 기본 단위는 `객체`를 지칭한다. 따라서 *객체는 하나의 책임만 가져야 한다*는 의미이다.

그렇다면 `책임`이란 무엇인가? 보통 책임은 `해야 하는 것`이나 `할 수 있는 것`으로 간주한다. 그리고 객체에 책임을 할당할 때는 어떤 객체보다도 작업을 `잘 할 수 있는` 객체에 책임을 할당해야 한다. 또한 객체는 책임에 수반되는 모든 일을 자신만이 수행해야 한다.

예를 들어 `학생` 클래스가 수강 과목을 추가하거나 조회하고, DB에 객체 정보를 저장하고 불러오는 작업을 처리하고, 성적표와 출석부에 출력하는 일까지 한다고 가정하자.

```java
public class Student {
    public void getCourses() { ... }
    public void addCourse(Course c) { ... }

    public void save() { ... }
    public Student load() { ... }

    public void printOnReportCard() { ... }
    public void printOnAttendanceBook() { ... }
}
```

이러한 경우 `학생` 클래스는 너무나 많은 책임을 수행해야만 한다. `단일 책임 원칙(SRP)`을 만족하기 위해서는 `학생` 클래스가 가장 잘할 수 있는 책임(수강 과목 추가/조회)만을 남겨두는 것이다. DB작업이나 성적표/출석부 출력의 경우 다른 클래스가 잘할 수 있는 여지가 많다.

### 변경

SRP를 따르는 실효성 있는 설계가 되려면 책임을 좀 더 현실적인 개념으로 파악할 필요가 있다. 우리가 설계 원칙을 학습하는 이유는 `예측하지 못한 변경사항에 유연하고 확장성 있는 시스템 구조`를 설계하기 위해서이다.

좋은 설계란 기본적으로 시스템에 새로운 요구사항이나 변경이 있을 때 가능한 한 영향 받는 부분을 줄여야 한다. 가령 어떤 클래스가 잘 설계되었는지를 판단하려면 `언제 변경`되어야 하는지를 물어보는 것이 좋다.

그렇다면 `학생` 클래스는 언제 변경되어야 하나?

- DB 스키마 변경 시?
- 학생이 지도 교수를 찾는 기능이 추가된다면?
- 새로운 형식으로 출력하고 싶다면?

이러한 사항은 모두 학생 클래스를 변경해야 하는 이유가 된다. 또한 책임을 많이 질수록 클래스 내부에서 서로 다른 역할을 수행하는 코드끼리 강하게 결합될 가능성이 높다.

### 책임 분리

`학생` 클래스는 여러 책임을 수행하므로 이것의 도움을 필요로 하는 코드가 많을 수 밖에 없다. 그렇기 때문에 `학생` 클래스에 변경사항이 생기면 직접 또는 간접적으로 연관되어 있는 모든 코드들을 다시 테스트해야 한다.

> 참고로 어떤 변화가 있을 때 기존 시스템 기능에 영향을 주는지 평가하는 테스트를 `회귀 테스트`라고 한다.

모든 코드를 테스트하지 않기 위해서는 한 클래스에 너무 많은 책임을 부여하지 말고 `단 하나의 책임`만 수행하도록 해서 변경 사유가 될 수 있는 것을 하나로 만들어야 한다. 이것을 `책임 분리`라 한다.

![advanced-design](/images/oop/solid-principle/advanced-design.jpg#center)

`학생` 클래스의 경우 변경 사유가 될 수 있는 것은 학생의 `고유 정보`, `DB 스키마`, `출력 형식`의 변화 등 3가지이다. 따라서 학생 클래스는 고유의 역할만 수행하고 DB작업은 DAO(Data Access Object) 클래스, 출석부와 성적표에 출력을 담당하는 클래스로 분리하는 것이 좋다.

### 산탄총 수술

지금까지는 한 클래스가 여러 가지 책임을 가진 상황을 살펴봤다. 반대로 `하나의 책임이 여러 클래스들로 분산`되어 있는 경우도 단일 책임 원칙에 입각해 설계를 변경해야 한다. 이러한 경우를 `산탄총 수술(shotgun surgery)`라고 한다.

하나의 책임이 여러 개의 클래스로 분리되어 있는 예는 `로깅, 보안, 트랜잭션`과 같은 `횡단 관심`으로 분류할 수 있는 기능이 대표적이다. 횡단 관심에 속하는 기능은 대부분 `시스템 핵심 기능` 안에 포함되는 `부가 기능`이다.

![cross-cutting-concern](/images/oop/solid-principle/cross-cutting-concern.jpg#center)

가령 시스템에서 실행하는 특정 메시지들의 실행 로그를 DB에 저장하는 코드가 있다고 할 때, 이것을 파일로 저장하게 변경한다면 로그 기능이 삽입된 메서드를 모두 찾아야만 한다.

이를 해결하는 방법은 이러한 부가 기능을 `별개의 클래스`로 분리해 책임을 담당하게 하는 것이다. 즉, 여러 곳에 흩어진 `공통 책임`을 한 곳에 모으면서 `응집도`를 높인다. 그러나 여전히 구현된 기능들을 호출하고 사용하는 코드는 해당 기능을 사용하는 코드 어딘가에 포함될 수밖에 없다. 

횡단 관심 문제를 해결하기 위한 방법으로 `관심지향 프로그래밍(AOP)`라는 기법이 있다. AOP는 횡단 관심을 수행하는 코드를 `애스펙트(aspect)`라는 특별한 객체로 모듈화하고, `위빙(weaving)`이라는 작업을 통해 모듈화한 코드를 핵심 기능에 끼워넣는다. 만약 횡단 관심에 변경이 생기면 기존 코드를 전혀 변경하지 않고 해당 애스팩트만 수정한다.

개방-폐쇄 원칙(OCP)
---

> `개방-폐쇄 원칙(Open-Closed Principle, OCP)`: 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다는 원칙

![case-that-violate-ocp](/images/oop/solid-principle/case-that-violate-ocp.jpg#center)

만약 도서관 대여 명부와 같은 새로운 매체에 학생의 대여 기록을 출력하는 기능을 추가하려면 어떻게 해야할까? `도서관 대여 명부` 클래스를 새로 만들어서 `Client` 클래스가 이 기능을 이용하면 될 것처럼 보인다. 하지만 이러한 방식은 OCP를 위반한다. 새로운 기능을 추가함에 따라서 client를 수정해야만 하기 때문이다.

![case-that-satisfy-ocp](/images/oop/solid-principle/case-that-satisfy-ocp.jpg#center)

새로운 기능을 추가하더라도 `Client` 클래스에 영향을 주지 않게 하려면 `개별적인 클래스`를 직접 접근하는 것이 아닌 `인터페이스`를 통해 구체적인 기능을 `캡슐화`하여 처리해야 한다.

*클래스는 변경하지 않고도(closed) 대상 클래스의 환경을 변경(open)할 수 있는 설계가 되어야 한다.*

리스코프 치환 원칙(LSP)
---

> `리스코프 치환 원칙(Liskov Substitution Principle, LSP)`: 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다는 원칙

일반화 관계는 다른 말로 `is-a-kind-of` 관계라고 한다. 예를 들어 원숭이와 포유류 사이에는 해당 관계가 성립한다(원숭이 is a kind of 포유류). 이때 부모 클래스로 `포유류`, 자식 클래스로 `원숭이`를 설정할 수 있다.

```plaintext
- 포유류는 새끼를 낳아 번식한다.
- 포유류는 젖을 먹여서 새끼를 키우고 폐를 통해 호흡한다.
- 포유류는 체온이 일정한 정온 동물이다.
```

위는 포유류의 여러 특징을 설명한 것이다.

```plaintext
- 원숭이는 새끼를 낳아 번식한다.
- 원숭이는 젖을 먹여서 새끼를 키우고 폐를 통해 호흡한다.
- 원숭이는 체온이 일정한 정온 동물이다.
```

`리스코프 치환 원칙`에 의해 자식 클래스는 부모 클래스에서 가능한 행위는 수행할 수 있다. 따라서 포유류의 특성은 원숭이도 마찬가지로 수행할 수 있다.

### 오리너구리는 포유류일까?

```plaintext
- 오리너구리는 새끼를 낳아 번식한다.
- 오리너구리는 젖을 먹여서 새끼를 키우고 폐를 통해 호흡한다.
- 오리너구리는 체온이 일정한 정온 동물이다.
```

이와 같은 상황을 `오리너구리`에 적용해보자. 오리너구리는 포유류임에도 새끼를 낳지 않고 알을 낳는 동물이다. 이러한 경우는 부모 클래스에서 가능한 행위를 수행하지 못한다. 따라서 위 `포유류에 대한 설명이 잘못되었다`고 결론을 내릴 수 있다.

*LSP를 만족하려면 `부모 클래스의 인스턴스`를 `자식 클래스의 인스턴스`로 대신할 수 있어야 한다.*

> 실제로 포유류를 분류하는 데에는 `태생`을 기준으로 두지 않는다. 참고로 알을 낳는 포유류를 `단공류`라 한다. 따라서 `단공류를 제외한 포유류는 태생이다`라는 명제는 참이다.

### 오버라이딩(override)

하지만 이러한 의문을 가질 수도 있을 것이다. 포유류 중에 알을 낳는 경우는 소수이므로 예외적인 경우만 `재정의(override)`해서 사용하면 안되는 걸까? 

```plaintext
- 오리너구리는 알을 낳아 번식한다. (override)
- 오리너구리는 젖을 먹여서 새끼를 키우고 폐를 통해 호흡한다.
- 오리너구리는 체온이 일정한 정온 동물이다.
```

만약 재정의를 사용하게 된다면 오리너구리는 위와 같이 정의할 수 있을 것이며, 실제로 코드는 잘 동작할 수 있다. 다만 아래와 같은 두 가지 OOP 규칙 위배가 발생한다.

- LSP를 만족하지 않음
    - 오리너구리 클래스의 구현은 포유류 클래스의 행위와 일관되지 않음
- 피터 코드의 상속 규칙 위반
    - `서브 클래스가 슈퍼 클래스의 책임을 무시하거나 재정의하지 않고 확장만 수행한다`라는 규칙

피터 코드의 상속 규칙에서 말하는 `재정의하지 않고 확장만 한다`는 말은 결국 오버라이드하지 않는다는 것과 같은 의미이다. 따라서, 피터 코드의 상속 규칙을 따르는 것은 LSP를 만족시키는 방법 중 하나이다. **Do Not Override!**

의존 역전 원칙(DIP)
---

> `의존 역전 원칙(Dependency Inversion Principle, DIP)`: 의존 관계를 맺을 때 변화가 어렵거나, 거의 변화가 없는 것에 의존하라는 원칙

사람이 음료를 마시는 경우를 가정해보자. 우리는 날마다 물을 먹기도 하지만 커피를 마시거나 콜라를 즐기기도 한다. 구체적으로 무엇을 마시는가는 변하기 쉬운 것이지만 무언가를 마신다는 사실 자체는 변하기 어렵다.

![apply-dip-to-beverage](/images/oop/solid-principle/apply-dip-to-beverage.jpg#center)

객체지향에서는 이와 같이 변화하기 어려운 추상적인 것을 표현하기 위해 `추상 클래스`나 `인터페이스`를 사용한다. DIP를 만족하기 위해서는 구체 클래스보다는 `인터페이스`나 `추상 클래스`와 의존 관계를 맺도록 설계해야 한다.

### 의존성 주입(DI)

`의존성 주입(Dependency Injection, DI)`란 클래스 외부에서 의존되는 것을 대상 객체의 인스턴스 변수에 주입하는 기술이다. 이것을 이용하면 대상 객체를 변경하지 않고도 외부에서 대상 객체의 외부 의존 객체를 바꿀 수 있다.

```java
public class Person {
    private Beverage beverage

    public void Person() {
        this.beverage = new Water();
        // this.beverage = new Coffee();
        // this.beverage = new Coke();
    }

    public void drink(){
        beverage.drink();
    }
}
```

우선 의존성 주입을 사용하지 않는 경우부터 살펴보겠다. 마시고 싶은 음료에 따라서 사람은 생성자에서 생성되는 음료를 선택할 수 있다. 하지만 위 같은 설계의 경우 마시는 음료가 변화함에 따라 생성자 코드를 변경해야만 한다.

```java
public class Person {
    private Beverage beverage

    public void setBeverage(Beverage beverage) {
        this.beverage = beverage;
    }

    public void drink(){
        beverage.drink();
    }
}
```

의존성 주입을 이용하는 경우 필요한 객체를 `클래스 내부`에서 직접 생성하는 것이 아니라 `외부에서 주입`하여 객체 간의 결합도를 줄이고 코드를 유연하게 할 수 있다. *의존성 주입은 외부에서 필요한 객체를 받아서 사용하는 것이다.*

인터페이스 분리 원칙(ISP)
---

> `인터페이스 분리 원칙(Interface Segregation Principle, ISP)`: 클라이언트 자신이 이용하지 않는 기능에는 영향을 받지 않아야 한다는 원칙

![multifunction-machine-class-diagram](/images/oop/solid-principle/multifunction-machine-class-diagram.jpg#center)

복합기의 경우를 생각해보자. 복합기는 프린트 기능뿐 아니라 다양한 기능을 복합적으로 사용가능한 장치이다. 그렇기 때문에 여러 클라이언트의 작업 요청을 처리할 수 있어야만 한다.

프린트 기능을 사용중인 클라이언트는 팩스나 복사 기능의 영향을 받아서는 안된다. 하지만 위와 같은 구조는 한 클래스 내에서 모든 기능을 구현하므로 관련 없는 기능에도 영향을 줄 가능성이 높다. 따라서 `인터페이스`를 통해 `클라이언트에 특화`되로록 분리시켜야 한다.

![apply-isp-to-multifunction-machine-class-diagram](/images/oop/solid-principle/apply-isp-to-multifunction-machine-class-diagram.jpg#center)

복합기를 사용하는 객체들마다 자신이 관심을 갖는 메서드들만 있는 `인터페이스`를 제공받도록 설계했다. 이렇게 설계하면 인터페이스가 일종의 `방화벽 역할`을 수행해 클라이언트는 자신이 `사용하지 않는` 메서드에 생긴 변화로 인한 영향을 받지 않게 된다.

### SRP와 ISP

어떤 클래스가 단일 책임을 수행하지 않고 `여러 책임`을 수행하게 되면 `방대한 메서드`를 가진 비대한 클래스가 될 것이다. 이러한 클래스를 `SRP`에 따라 단일 책임을 갖는 여러 클래스들로 분할하고 각자의 인터페이스를 제공한다면 `ISP도 만족`할 수 있다.

그렇다면 SRP는 ISP 만족을 위한 필요조건인가? 그렇다고만 할 수는 없다. 가령 게시판의 기능을 제공하기 위한 클래스가 있다고 있다고 하자. 이 클래스가 `CRUD` 메서드를 구현하고 있다면, 게시판에 관련된 책임을 수행하는 것이므로 `SRP를 만족`한다고 볼 수 있다.

그러나 클라이언트에 따라서 게시판의 `일부 기능`만 사용하도록 제한될 수 있다. 글을 삭제하는 권한이 관리자에게만 있는 경우와 같이 말이다. 이 클래스의 모든 메서드가 들어 있는 인터페이스가 `클라이언트와 상관없이 사용`된다면 `ISP에 위배`된다.

참고문헌
---

- [정인성, 채흥석,『JAVA 객체지향 디자인 패턴』, 한빛미디어](http://www.yes24.com/Product/Goods/12501269)
- [위키피디아, 포유류](https://ko.wikipedia.org/wiki/%ED%8F%AC%EC%9C%A0%EB%A5%98)
- [wlsdud2194, [DI] Dependency Injection이란 무엇일까?, velog](https://velog.io/@wlsdud2194/what-is-di)
