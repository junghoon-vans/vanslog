---
title: "[Java] 기본 자료형과 참조 자료형"
date: 2021-07-16T14:06:26Z
series:
- 자바 스터디
categories:
- Java
tags:
- Java
- 자료형
- 기본 자료형
- 참조 자료형
draft: false
description: >
    자바의 기본 자료형과 참조 자료형에 대해서 알아보자!
---

자료형 종류
---

![java data types](http://www.btechsmartclass.com/java/java_images/java-data-types.jpg)

자료형은 크게 `기본 자료형`과 `참조 자료형`으로 나누어진다. 이번 포스팅에서는 자바 프로그래밍에 있어서 기초가 되는 자료형에 한해서만 알아보기로 한다. 세부적인 내용은 추후 포스팅에서 다뤄보도록 하겠다.

### 기본 자료형(Primitive Type)

`기본형`은 우리가 흔히 알고 사용하는 `int`, `float`, `char`, `boolean`과 같은 자료형이다. 자바는 정적 타이핑으로 작성되는 언어이므로, 기본 자료형에 대해서 잘 알고 있어야 한다. 사용 목적에 맞는 자료형을 적재적소에 배치하는 것은 효율성 좋은 코드를 짤 수 있게 한다.

| 자료형 | 의미 | 메모리 사이즈 | 범위 | 기본값 |
|--------|------|---------------|------|--------|
| byte | 8-bit 정수 | 1 byte | -2⁷ ~ 2⁷-1 | 0 |
| short | 16-bit 정수 | 2 bytes | -2¹⁵ ~ 2¹⁵-1 |0 |
| int | 32-bit 정수 | 4 bytes | -2³¹ ~ 2³¹-1 | 0 |
| long | 64-bit 정수 | 8 bytes | -2⁶³ ~ 2⁶³-1 | 0L |
| float | 32-bit 부동소수점(IEEE 754) | 4 bytes | -3.40E+38 ~ 3.40E+38 | 0.0f |
| double | 64-bit 부동소수점(IEEE 754) | 8 bytes | 1.79E+308 ~ 1.79E+308 | 0.0d |
| char | 16-bit 유니코드 문자 | 2 bytes | 0 ~ 2¹⁶-1 | \u0000 |
| boolean | 논리형 | 1 bit | 0 or 1 | 0 (false) |

> C/C++과 달리 자바는 정수형에서 unsigned형이 없다. 엄연히 따지면 java 8 이후부터는 `int`, `long`형에 한에서는 `Wrapper Class`를 통해 몇가지 정적 메서드를 사용할 수 있도록 하고 있다.

### 참조 자료형(Reference Type)

`참조형`은 `기본 자료형`을 기초로 하여 만들어진 자료형이다. 대표적으로 자바에서 제공하는 String, Array, Map, Set 등과 같은 `클래스(Class)`와 `인터페이스(Interface)`, `열거형(Enum)`이 여기에 해당한다. 추가적으로 필요에 따라 사용자가 참조형 타입을 정의할 수도 있다.

#### Object

모든 `Class`와 `Enum`은 `Object` 클래스를 상속한다. 다시말해 Object는 모든 Class와 Enum의 일반화된 타입이라는 것이다. 여기서 주의할 점은 Interface는 Object를 상속하지 않는다는 사실이다. 이와 관련된 내용은 자바 api 문서 [Tree](https://docs.oracle.com/en/java/javase/16/docs/api/overview-tree.html)에 잘 드러나 있다.

> [자바 api 문서](https://docs.oracle.com/en/java/javase/16/docs/api/index.html)에는 제공하는 모든 `Class`, `Interface`, `Enum` 등의 상세 스펙을 정의하고 있다. 이 사이트는 자바를 깊이 이해하는데 많은 도움을 주므로 자주 방문하면 좋다. 다만 자바 버전에 따라 api구현이 상이하므로 주의해야 한다.

#### String

`String`은 기본 자료형이 아니라는 사실은 익히 잘 알려져 있다. String은 `char`의 배열로 구현된 참조 자료형이다.

> `C/C++`의 경우는 문자열을 사용하기 위해서 실제로 char형의 배열을 직접 사용한다. 

자바가 `C/C++`과 달리 `String`형을 따로 제공하는 이유는 문자열에 `유용한 메서드`를 제공하기 위해서이다. charAt, concat, equals, indexOf, split와 같은 메서드를 이용하면 문자열을 쉽게 조작할 수 있다.

#### Array

`배열` 또한 마찬가지로 기본 자료형이 아니다. 이는 다음과 같은 간단한 코드를 통해서 테스트 해볼 수 있다.

```java
int[] array = new int[10];
System.out.println(array instanceof Object);
// true
```

모든 클래스는 Object의 상속 클래스이므로 위 코드를 통해 배열이 기본형이 아님을 알 수 있다. 만약 배열이 기본형이었다면 위와 같은 코드의 실행 결과 `false`가 나와야 한다.

#### Wrapper 클래스

`Wrapper Class`는 기본 자료형을 감싼 클래스이다. 대표적으로 Byte, Short, Integer, Long, Float, Double, Character, Boolean이 있다. 이것을 사용하는 이유는 앞서 설명한 `String`을 사용하는 이유와도 동일하다. 기본 자료형을 `클래스`로 랩핑하면 얻을 수 있는 이점은 `유용한 메서드를 제공`할 수 있다는 점이다.

하지만 이보다 더 중요한 이유는 `제네릭(Generic)`에 있다. 제네릭에 사용되는 매개변수 `T`는 `Object` 자료형만 받을 수 있다. 이것은 다시 말해 클래스로 정의된 객체만을 전달받는다는 것이다. 다만 코드를 짜다보면 제네릭을 기본 자료형에 적용해야 하는 경우가 있다. 이러한 경우에는 `Wrapper Class`을 이용하면 문제가 해소된다.

> 제네릭을 통한 `유연한 프로그래밍`을 기본 자료형에도 적용할 수 있게 하기 위해 `Wrapper Class`를 제공한다고 생각하면 이해가 쉬울 것이다.

### 차이점

앞서 기본 자료형과 참조 자료형이 무엇인지, 그리고 어떠한 차이가 있는지에 대해서 알아보았다. 이들의 차이에 대해서 간단히 알아보고 이번 포스팅을 마치려고 한다.

- 참조 자료형은 기본 자료형과 달리 메서드를 가질 수 있다.
- 참조 자료형의 기본값은 `null`이다.
  - 따라서 참조형 객체가 초기화되지 않으면 `nullPointerException`이 발생한다.
- 기본 자료형의 기본값은 위 [표](#기본-자료형primitive-type)를 참고하자.

참고문헌
---

- [btechsmartclass - java data types](http://www.btechsmartclass.com/java/java-data-types.html)
- [oracle - java documentation](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
