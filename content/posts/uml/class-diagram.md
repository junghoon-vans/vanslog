---
title: "[UML 2.0] 클래스 다이어그램(Class Diagram)"
date: 2021-01-08T19:20:43+09:00
series:
  - UML 설계
categories:
  - UML
tags:
  - UML
  - OOP
  - ☕️ Java
  - 클래스
  - 모델링
draft: false
---

클래스란?
---

클래스는 동일한 속성과 행위를 수행하는 객체들의 집합이다. 예를 들어 소프트웨어를 전공중인 학생들의 공통점은 `소프트웨어를 전공한다`는 사실과 `동일한 전공 수업`을 듣는다는 점이다. 이러한 경우 `소프트웨어 전공 중인 학생`은 실제 학생들의 클래스라고 말할 수 있다.

클래스를 정의하는 또 다른 관점은 `인스턴스(객체)를 생성하는 설계도`로 보는 것이다. 아래 소스코드는 소프트웨어를 전공하는 학생 클래스를 정의한 것이다. 클래스가 객체를 생성하는 설계도라는 관점에서 코드를 한 번 살펴보자

```java
public class Student{
    private String name;
    private String major = "SW";

    public Student(String name){
        this.name = name;
    }

    public void study() {
        System.out.println("객체 지향 모델링 수업을 수강합니다.");
    }
}
```

```java
Student student1 = new Student("학생1");
Student student2 = new Student("학생2");

student1.study();
student2.study();
```

위 코드를 통해서 같은 스펙을 가진 학생 객체가 두 개 생겨났다. 두 객체는 이름을 제외하면 모두 동일한 특성을 가진다. major 속성은 `SW`로 동일하며 `study()` 메소드를 실행하면 동일한 문장이 출력된다. 이것이 바로 클래스가 객체를 생성하는 설계도인 이유이다.

> 동일한 설계도에서는 항상 동일한 객체가 생성된다.

UML 모델링
---

![student-uml-class](/images/uml/class-diagram/student-uml-class.png#center)

`UML`을 이용해서 `Student` 클래스를 표기한 결과는 위와 같다. 가장 위에서부터 구획별로 `클래스명(ClassName)`, `속성(Property)`, `연산(Operation)`을 기술한다. 만약 속성이나 연산이 없다면 생략할 수 있다.

### 접근 제어자

| 접근 제어자 | 표시 | 설명 |
|-------------|------|------|
| public | + | 어떤 클래스의 객체든 접근 가능 |
| private | - | 이 클래스에서 생성한 객체들만 접근 가능 |
| protected | # | 이 클래스와 동일 패키지에 있거나, 상속 관계에 있는 하위 클래스의 객체들만 접근 가능 |
| package | ~ | 동일 패키지에 있는 클래스의 객체들만 접근 가능 |

클래스의 속성과 연산을 정의할 때 `-`나 `+`와 같은 기호를 사용하는데, 이것은 `가시화`를 정의하는 접근 제어자이다. 자바에서의 `private`과 `public`를 위와 같이 표기한다고 생각하면 된다.

### 속성과 연산

| 구분 | 표기법 |
|------|--------|
| 속성 | [접근 제어자]이름: 타입[다중성 정보] [=초기값] |
| 연산 | [접근 제어자]이름(인자: 타입): 리턴 타입 |

속성과 연산을 표기하는 형식은 위와 같다. 클래스 다이어그램은 개념 분석 단계에서 구현에 이르기까지 광범위하게 사용되는데, `분석 단계`에서는 속성이나 연산을 구체적으로 표현하기 보다는 정의하는 것이 주를 이룬다. 이후 `설계 단계`에서 구체적인 타입 정보나 가시화 정보를 기술하게 된다.

> `[]` 안에 들어있는 것은 생략해도 괜찮다는 의미이다.

관계
---

객체 지향 프로그래밍에서 객체 하나만을 사용하는 경우는 드물다. 보통 기능별로 객체를 나누어 지고 이들의 상호작용이 하나의 소프트웨어를 동작하게 한다. 이러한 클래스 간의 관계를 `UML`에서는 아래와 같이 표현한다.

### 연관 관계

클래스들이 개념상 연결되어 있음을 의미하며 실선으로 표시한다.

![uml-association-notation](/images/uml/class-diagram/uml-association-notation.jpg#center)

#### 양방향 연관 관계

교수와 학생 클래스의 연관 관계와 같이 `서로를 인식`하는 경우를 양방향 연관 관계라고 하며, `화살표 없는` 실선으로 표기한다. 

![professor-student-relation](/images/uml/class-diagram/professor-student-relation.png#center)

만약 이들이 상담한다는 것을 나타내고 싶다면 위와 같이 실선 `상단`에 명시해주면 된다. 연관 관계에서의 `역할 이름(rule name)`도 정할 수 있는데 실선의 `양 끝단`에 정의해주면 된다. 이것은 이후 프로그램을 구현하는 단계에서 서로를 참조하는 `속성`으로 활용될 수 있다.

#### 단방향 연관 관계

학생 한 명은 여러 수업을 수강할 수 있다. 이것을 UML을 통해서 모델링한다면 다음과 같이 표기할 수 있다. 

![student-course-relation](/images/uml/class-diagram/student-course-relation.png#center)

이때 화살표는 학생에서 수업으로 향하는데 이는 `학생이 수업을 인식`하고 있음을 의미하며, 반대로 수업은 학생을 인식하지 못한다. 이러한 경우를 `단방향 연관 관계`라고 한다.

위 다이어그램에는 `1..*`이라는 표기가 있는데 이것은 `다중성`을 나타낸 것이다. 다중성은 `연관되어 있는 객체의 수`를 의미한다. 

> `*`은 `0 이상`을 의미하며, `..`은 범위를 나타낸다. 따라서 `1..*`는 `1 이상`이라는 의미이다. 객체가 하나인 경우는 생략하기도 한다.

#### 다대다 연관 관계

앞선 예시를 잘 생각해보면 뭔가 이상하다는 사실을 깨닫을 수 있다. 실세계에서는 한 명의 학생만이 수업을 수강하는 경우는 없다. 보통은 다수의 학생이 다수의 수업을 수강한다. 이것을 표현하면 아래와 같은 그림이 나올 것이다.

![many-to-many-relation](/images/uml/class-diagram/many-to-many-relation.png#center)

이렇게 `다수의 객체 - 다수의 객체`가 가지는 관계를 `다대다 연관 관계`라 하며, 이것은 일반적으로 UML에서 `양방향 연관 관계`로 표현된다.

![association-class](/images/uml/class-diagram/association-class.jpg#center)

그런데 만약에 학생이 수업을 수강하면서 발생하는 성적 정보를 저장하고 싶다면 어디에 저장해야 할까? 학생이나 수업 클래스에 그대로 성적 정보를 저장한다면 다음과 같이 표현될 것이다. 

`홍길동 학생이 A+이다` 또는 `객체 지향 모델링 수업에서 A+을 받았다`

다만 여기에는 어떤 수업에서 누가 해당 성적을 얻었는가에 대한 정보가 빠져 있다. 따라서 학생 성적은 학생이나 수업 클래스에 저장하는 것보다는 `별도의 클래스`를 만들어 저장하는 것이 옳다. 이때 사용되는 `Transcript`와 같은 클래스를 `연관 클래스`라 한다.

![association-class-to-general-class](/images/uml/class-diagram/association-class-to-general-class.jpg#center)

`연관 클래스`의 실제 구현은 `일반 클래스`의 `단방향 연관 관계`로 변환되어 이루어진다.

> 실제로 프로그램을 구현할 때, `양방향 연관 관계`는 사용되지 않는다!

#### 재귀적 연관 관계

![superior-subordinate-relation](/images/uml/class-diagram/superior-subordinate-relation.jpg#center)

연관 관계는 때로는 재귀적이다. 예를 들면 군대에는 `선임`과 `후임`이라는 관계가 존재한다. 내게 선임인 군인도 누구에게는 후임이며, 내게 후임인 군인도 누군가에겐 선임이다. 

![recursive-association](/images/uml/class-diagram/recursive-association.jpg#center)

이러한 경우 군인이라는 클래스는 선임과 후임이라는 두 클래스에 동시에 속하는 모순이 발생한다. 하지만 그렇다고 해서 두 클래스를 별도로 만드는 것은 유연성이 부족하다. 이러한 경우 `재귀적 연관 관계`가 사용된다.

하지만 재귀적 연관 관계에는 `관계의 루프`라는 문제가 남아 있다. 예를 들어 `가위바위보`는 가위가 보를 이기고 보는 바위를 이기고 바위는 가위를 이기는 게임이다. 이렇게 루프가 존재하는 경우는 `{계층}`으로 제약을 설정하여 배제해야만 한다.

> `{계층}`은 객체 사이에는 `상하 관계`가 존재하며, `사이클`이 존재하지 않음을 의미한다.

### 일반화 관계

한 클래스가 다른 클래스를 포함하는 `상위 개념`일 때 두 클래스 간의 관계이다.

![uml-generalization-notation](/images/uml/class-diagram/uml-generalization-notation.jpg#center)

`자식 클래스(서브 클래스)`는 `부모 클래스(슈퍼 클래스)`로부터 속성이나 연산을 물려 받을 수 있다. 그렇기 때문에 일반화 관계를 `상속 관계`라고도 한다.

![home-appliance-inheritance](/images/uml/class-diagram/home-appliance-inheritance.jpg#center)

보통 일반화 관계는 `is-a-kind-of` 관계라고 말한다. 가전 제품과 세탁기의 관계는 `세탁기 is-a-kind-of 가전 제품`라고 설명할 수 있다.

### 집합 관계

집합 관계는 연관 관계의 특별한 경우로 전체와 부분의 관계를 명확하게 명시하고자 할 때 사용한다. `집약(aggregation)`과 `합성(composition)` 두 종류의 집합 관계가 존재한다.

#### 집약 관계

한 객체가 다른 객체를 포함하는 것을 나타낸다.

![uml-aggregation-notation](/images/uml/class-diagram/uml-aggregation-notation.jpg#center)

- 전체를 가리키는 클래스 방향에 빈 마름모 표시
- 부분 객체를 `다른 객체와 공유`할 수 있음
- 전체 객체와 부분 객체의 `라이프타임`은 독립적

#### 합성 관계

부분 객체가 전체 객체에 속하는 관계이다.

![uml-composition-notation](/images/uml/class-diagram/uml-composition-notation.jpg#center)

- 전체를 가리키는 클래스 방향에 채워진 마름모 표시
- 부분 객체를 `다른 객체와 공유`할 수 없음
- 부분 객체의 `라이프타임`은 전체 객체에 의존

#### 차이점

집약 관계와 합성 관계는 얼핏보면 비슷해 보이지만 큰 차이를 가지고 있다. 가장 중요한 차이는 `라이프타임`이다. 전체 객체가 소멸되었을 때 `부분 객체가 남아 있다면` 그것은 `집약 관계`이다. 반대의 경우는 `합성 관계`라고 생각하면 된다.

컴퓨터를 조립하는 예시를 가지고 이 둘의 차이를 알아 보겠다. 

![computer-components-aggregation](/images/uml/class-diagram/computer-components-aggregation.jpg#center)

```java
public class Computer {
    private MainBoard mainBoard;
    private CPU cpu;
    private Memory memory;
    private PowerSupply powerSupply;

    public Computer(MainBoard mainBoard, CPU cpu, Memory memory, PowerSupply powerSupply){
        this.mainBoard = mainBoard;
        this.cpu = cpu;
        this.memory = memory;
        this.powerSupply = powerSupply;
    }
}
```

컴퓨터 객체는 외부에서 만들어진 메인보드, CPU, 메모리, 파워서플라이를 받아서 생성된다. 따라서 컴퓨터 객체가 소멸되더라도 컴퓨터를 구성하는 부분 객체들은 사라지지 않고 메모리에 남아 있게 된다. 따라서 위 소스코드는 `집약 관계`를 나타낸 것이라는 사실을 알 수 있다.

![computer-components-composition](/images/uml/class-diagram/computer-components-composition.jpg#center)

```java
public class Computer {
    private MainBoard mainBoard;
    private CPU cpu;
    private Memory memory;
    private PowerSupply powerSupply;

    public Computer(){
        this.mainBoard = new MainBoard();
        this.cpu = new CPU();
        this.memory = new Memory();
        this.powerSupply = new PowerSupply();
    }
}
```

이전 예시와 달리 위 코드는 컴퓨터를 생성하는 동시에 구성품들이 생성된다. 따라서 해당 요소들의 생명주기는 전체 객체에 의존하게 된다. 컴퓨터가 소멸되는 동시에 부분 객체들도 사라지기 때문에 `합성 관계`라고 볼 수 있다.

### 의존 관계

다른 클래스에서 제공하는 기능을 사용할 때 나타나는 관계이다.

![uml-dependency-notation](/images/uml/class-diagram/uml-dependency-notation.jpg#center)

일반적으로 한 클래스가 다른 클래스를 사용하는 경우 다음 3가지이다.

- 클래스의 속성에서 참조
- 연산의 인자로 사용
- 메서드 내부의 지역 객체로 참조

![dependency-relation](/images/uml/class-diagram/dependency-relation.jpg#center)

사람이 차를 소유하고 있고, 이 차는 주유소에서 충전한다는 것을 다이어그램으로 나타내면 위와 같다. 이때, `사람-차` 간의 관계는 `연관 관계`이지만 `차-주유소` 간의 관게는 `의존 관계`이다.

```java
public class Person {
    private Car car;

    public void setCar(Car car){
        this.car = car;
    }
    ...
}

public class Car {
    public void fillGas(GasPump p){
        p.getGas(amount);
        ...
    }
}
```

사람이 타고 다니는 차는 매번 변화하는 것이 아니므로 `Person`클래스의 속성으로 `Car`객체를 참조한다. 반면 차를 주유하는 주요소는 매번 같지 않으므로 `인자`나 `지역 객체`를 통해 구현한다.

### 실체화 관계

인터페이스와 이것의 `책임`들을 실체화한 클래스 간의 관계이다.

> 책임이란 `객체가 해야 하는 일` 내지 `할 수 있는 일`을 말한다.

![uml-realization-notation](/images/uml/class-diagram/uml-realization-notation.jpg#center)

인터페이스 자체는 실제로 책임을 수행하는 객체가 아니다. 실체화를 통해서 만들어진 객체가 인터페이스에 정의된 책임을 수행하게 된다.

![flyable-interface](/images/uml/class-diagram/flyable-interface.jpg#center)

예를 들어 날기 위한 책임을 담은 `Flyable`이라는 인터페이스를 실체화한 `Plane`과 `Bird` 클래스는 해당 인터페이스의 책임을 구현해야만 한다. 이러한 점에서 인터페이스는 어떤 공통되는 능력이 있는 것들을 대표한다는 관점으로 볼 수도 있다. 그렇기 때문에 실체화 관계를 `can-do-this` 관계라고 부른다.

참고문헌
---

- [정인성, 채흥석,『JAVA 객체지향 디자인 패턴』, 한빛미디어](http://www.yes24.com/Product/Goods/12501269)
