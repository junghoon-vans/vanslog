---
title: "객체지향 프로그래밍(OOP) 특징"
date: 2021-01-09T17:18:32+09:00
series:
- 객체지향 개념
categories:
- OOP
tags:
- OOP
- Java
draft: true
description: >
    객체지향 프로그래밍(OOP)의 특징
---

추상화(Abstraction)
---

구체적인 사물들의 `공통적인 특징`을 파악해서 하나의 개념으로 다루는 수단이다. 즉 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념 또는 기능을 간추려내는 것이다.

이러한 추상화는 `객체지향 프로그래밍(OOP)`에서 클래스를 만들기 위해 필수적인 요소이다. 만약 추상화가 없다면 모든 객체를 세부적으로 정의하여 구현해야만 한다. 

캡슐화(Encapsulation)
---

`캡슐화`는 정보은닉을 통해 `높은 응집도`와 `낮은 결합도`를 갖도록 하는 객체지향 설계 원리이다.

### 응집도와 결합도

클래스의 디자인은 재사용 가능하고, 확장 가능하며 유지 보수가 용이해야 한다. 이를 위해서는 `결합도`는 낮아야 하며, `응집도`은 높아야 한다. 

- 응집도(Cohesion)
  - 클래스나 모듈 안의 요소들이 밀접하게 관련되어 있는 정도
- 결합도(Coupling)
  - 어떤 기능을 실행하는 데 다른 클래스나 모듈들에 의존적인 정도

### 정보 은닉(information hiding)

알 필요 없는 정보는 외부에서 접근하지 못하도록 제한하는 것이다. 우리는 자동차가 어떻게 동작하는 지 몰라도 운전을 할 수 있으며, 컴퓨터가 동작하는 원리를 몰라도 잘 이용할 수 있다.

```java
public class ArrayStack {
    public int top;
    public int[] itemArray;
    public int stackSize;

    public ArrayStack(int stackSize) {
        itemArray = new int[stackSize];
        top = -1;
        this.stackSize = stackSize;
    }

    public void push(int item) { // item 추가
        ...
    }

    public int pop() { // item 반환
        ...
    }

    public int peak() { // item 출력
        ...
    }
    ...
}
```

`ArrayStack`은 배열을 사용해서 구현된 스택이다. `push` 함수를 이용해서 아이템을 추가하고 `pop`을 통해서 반환할 수 있다.

```java
public class StackClient {
    public static void main(String[] args){
        ArrayStack st = new ArrayStack(10);
        st.itemArray[++st.top] = 20;
        System.out.print(st.itemArray[st.top]);
    }
}
```

하지만 ArrayStack의 속성은 모두 `public`으로 지정되어 있어, 외부에서도 접근이 가능하다. 이러한 경우 `push`함수를 이용하지 않고 직접 배열을 다룰 수 있게 되며, `StackClient`와 `ArrayStack` 사이에는 강한 결합이 발생한다.

만약 스택의 구현이 변경되는 경우 `StackClient`의 소스코드도 변경해야 할 수 있다. 그렇기 때문에 직접 스택의 속성을 다루는 것은 피해야 한다. 따라서 `ArrayStack`의 속성들은 접근 제어자를 `private`으로 설정해서 외부 클래스가 접근할 수 없도록 막는다. 이제 더 이상 직접 배열을 조작할 수 없게 되었으므로 `StackClient`는 아래와 같이 수정되어야 한다.

```java
public class StackClient {
    public static void main(String[] args){
        ArrayStack st = new ArrayStack(10);
        st.push(20);
        System.out.print(st.peek());
    }
}
```

일반화(Generalization)
---

여러 개체들이 가진 공통된 특성을 부각시켜 하나의 개념이나 법칙으로 성립시키는 과정이다.

- 일반화 관계는 객체지향 프로그래밍 관점에서 상속 관계라고 한다.
- 따라서 속성이나 기능의 재사용만 강조해서 사용하는 경우가 많다.
- 이는 일반화 관계를 극히 한정되게 바라보는 시각이다.

### 캡슐화를 위한 일반화

일반화는 외부 세계에 자식 클래스를 `캡슐화(또는 은닉)`하는 개념으로 볼 수도 있다.

{{< img src="images/posts/oop/oop-features/generalization-is-encapsulation.jpg" alt="generalization-is-encapsulation">}}

예를 들어 우리는 자동차를 구분할 때 BMW, 현대, 벤츠와 같은 여러 제조사들로 구분한다. 대리 운전 기사가 운전을 하는 상황을 가정해보면 차종은 운전에 큰 영향을 주지 않는다. `사람` 클래스 관점에서는 구체적인 자동차가 아닌 `자동차` 클래스만 관심을 가지면 된다. 따라서 구체적인 자동차 클래스는 `은닉`되어 있다고 볼 수 있다.

### 집합론 관점에서 일반화

일반화 관계는 `집합론`적인 관점에서 해석할 수도 있다.

{{< img src="images/posts/oop/oop-features/generalization-as-set-theory.jpg" alt="generalization-as-set-theory">}}

부모 클래스 A는 전체 집합 A에 해당하고 그 부분 집합 A1, A2, A3는 각각 A의 자식 클래스에 해당한다. 이때 다음 관계가 성립되어야 한다.

- `A = A1 ∪ A2 ∪ A3`
- `A1 ∩ A2 ∩ A3 = ø`

그리고 아래와 같은 제약 조건도 존재한다.

{{< img src="images/posts/oop/oop-features/constraints-in-generalization-relationships.jpg" alt="constraints-in-generalization-relationships">}}

- `{disjoint}`: 자식 클래스 객체가 동시에 두 클래스에 속할 수 없다
- `{complate}`: 자식 클래스의 객체에 해당하는 `부모 클래스의 객체`와 부모 클래스의 객체에 해당하는 `자식 클래스의 객체`가 하나만 존재

집합론 관점에서 일반화 관계를 만들면 연관 관계를 단순하게 할 수 있다.

{{< img src="images/posts/oop/oop-features/association-in-web-shop-member.jpg" alt="association-in-web-shop-member">}}

만약 인터넷 쇼핑몰에서 `VIP 고객`과 `일반 고객`을 분류하고 있다고 한다면 위와 같은 다이어그램을 그릴 수 있을 것이다. 각 회원은 각각 물건과 연관 관계를 맺게 할 수 있지만, 물건을 구매하는 것은 등급과는 무관하다.

{{< img src="images/posts/oop/oop-features/use-set-theory-to-simplify-association.jpg" alt="use-set-theory-to-simplify-association">}}

다시 말해 물건 클래스와의 연관 관계는 물건 클래스의 자식 클래스가 가지는 `공통적인 연관 관계`이다. 따라서 물건 클래스를 회원 클래스와 연관을 갖게 하여 다이어그램을 `간결화`할 수 있다,

> 집합론적인 관점에서의 일반화는 상호 배타적인 부분 집합으로 나누는 과정으로 간주할 수 있다.

만약 회원을 구분하는 기준이 추가된다면 어떻게 해야 할까? 웹 쇼핑몰에서는 국내 회원만 아니라 `외국의 회원`에게도 서비스를 제공하는 경우가 있다. 회원의 국가에 따라서 다른 서비스를 제공한다고 할 때, 이러한 구분은 꼭 필요할 것이다. 

UML에서는 이러한 구분을 `변별자`라 하며 일반화 관계를 표시하는 선 옆에 변별자 정보를 표시한다. 하지만 이러한 경우 회원은 구 가지 기준(결재금액, 지역)에 따라 구분되게 되는데, 이와 같이 한 인스턴스가 동시에 여러 클래스에 속할 수 있는 것을 `다중 분류`라고 한다. `<<다중>>` 스테레오 타입을 사용해서 표현한다.

{{< img src="images/posts/oop/oop-features/discriminator-and-multiple-classification.jpg" alt="discriminator-and-multiple-classification">}}

일반적으로 각 변별자에 따른 일반화 관계가 완전히 독립적이라면 별다른 문제가 없다. 하지만 요구사항의 변경/추가로 인해 두 `일반화 관계가 독립적이지 않은 상황`도 고려해야 한다.

예를 들어 위 다이어그램에서 `VIP 회원`에게 할인 쿠폰을 제공한다는 것은 쉽게 구현이 가능하지만, `일반 등급인 외국 회원`에게 선물을 제공한다는 것은 불가능하다.

{{< img src="images/posts/oop/oop-features/classes-for-all-combinations.jpg" alt="classes-for-all-combinations">}}

이를 처리하기 한 가지 방법은 `모든 분류 가능한 조합에 대응하는 클래스`를 만드는 것이다.

다형성(Polymorphism)
---

서로 다른 클래스의 객체가 같은 메시지를 받았을 때 각자의 방식으로 동작하는 능력이다.

앞서 [캡슐화를 위한 일반화](#캡슐화를-위한-일반화)에서 보았던 자동차 사례를 생각해보자. 우리는 차의 브랜드와는 무관하게 운전하는 것이 가능하다. 그래서 `사람` 클래스의 관점에서는 `자동차` 클래스만을 관심을 가지면 된다고 했다. 이것이 가능한 이유는 `다형성`이 있기 때문이다.

```java
abstract class Car {
    public abstract void ride();
}

public class BMW extends Car {
    public void ride() { ... }
}

public class HYUNDAI extends Car {
    public void ride() { ... }
}

public class BENZ extends Car {
    public void ride() { ... }
}

public class Main {
    public static void rideCars(Car[] cars) {
        for(Car car: cars){
            car.ride();
        }
    }

    public static void main(String[] args){
        Car[] cars = {new BMW(), new HYUNDAI(), new BENZ()};
        rideCars(cars);
    }
}
```

다형성을 사용하는 경우 구체적으로 현재 어떤 클래스 객체가 참조되는지와 무관하게 프로그래밍할 수 있다. 따라서 새로운 자동차 클래스가 자식 클래스로 추가되더라도 코드는 영향을 받지 않는다.

이것이 가능한 이유는 일반화 관계에 있을 때 부모 클래스의 참조 변수가 `자식 클래스의 객체를 참조`할 수 있기 때문이다. 단, 부모 클래스의 참조 변수가 접근할 수 있는 것은 부모 클래스가 `물려준 변수와 메서드`뿐이다.

피터 코드의 상속 규칙
---

`피터 코드`는 상속의 오용을 막기 위해 상속의 사용을 엄격하게 제한하는 규칙들을 만들었다.

- 자식 클래스와 부모 클래스 사이는 `역할 수행` 관계가 아니어야 한다.
- 한 클래스의 인스턴스는 다른 서브 클래스의 객체로 변환할 필요가 절대 없어야 한다.
- 자식 클래스가 부모 클래스의 책임을 무시하거나 재정의하지 않고 `확장(extends)`만 수행해야 한다.
- 자식 클래스가 단지 일부 기능을 재사용할 목적으로 `유틸리티 역할을 수행하는 클래스`를 상속하지 않아야 한다.
- 자식 클래스가 `역할`, `트랜잭션`, `디바이스` 등을 특수화해야 한다.

참고문헌
---

- [정인성, 채흥석,『JAVA 객체지향 디자인 패턴』, 한빛미디어](http://www.yes24.com/Product/Goods/12501269)
