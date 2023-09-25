---
title: "Java 21 패턴 매칭 제대로 활용하기(with Scala)"
date: 2023-09-21T22:27:53+09:00
series:
  - ☕️ Java 스터디
categories:
  - ☕️ Java
tags:
  - ☕️ Java
draft: false
summary: >
    고도로 발달된 Java는 Scala와 구분할 수 없다.
---

2023년 9월 19일, 그토록 기다리던 Java 21가 출시되었습니다. 대부분의 사람들의 관심이 `Virtual Thread`으로 집중되어 있는 한편 저는 `매턴 매칭(Pattern Matching)`에 눈길이 갔습니다. 현재 `오픈소스 컨트리뷰션 아카데미`에서 `ZIO` 팀의 멘티로 활동하며 해당 기능을 자주 활용한 경험이 있기 때문입니다.

> ZIO는 Scala의 비동기 동시성 라이브러리입니다.

그래서 이번 포스팅에서는 ZIO 스터디에서 진행하였던 예시를 통해 `패턴 매칭`이 무엇인지에 대해 알아보고, Java에 패턴 매칭을 구현하기 위한 프로젝트인 `Project Amber`에 대해 소개해드리고자 합니다.  끝으로는 Scala와 동일한 예시를 Java 21의 패턴 매칭을 활용하여 구현해보겠습니다.

# Pattern Matching with Scala

`Pattern Matching(매턴 매칭)`은 식에 대한 패턴을 체크하는 기능입니다. 이번 챕터에서는 [Tour of scala](https://docs.scala-lang.org/tour/pattern-matching.html)의 예시와 함께 패턴 매칭이 무엇인지에 대해서 간략하게 알아보겠습니다.

## 기본 문법

Scala에서는 Pattern Matching을 위해 다음과 같은 키워드를 제공합니다.

- `match`: 패턴 매칭을 시작하는 키워드
- `case`: 분기를 정의하는 키워드

```scala
import scala.util.Random

val x: Int = Random.nextInt(10)

x match
  case 0 => "zero"
  case 1 => "one"
  case 2 => "two"
  case _ => "other"
```

위 예시는 0부터 9까지의 `랜덤한 값`이 들어있는 변수 x를 선언하고, x의 값에 따라 분기합니다.
자바의 switch 문과 유사하며 `default` 키워드 대신 `언더스코어(_)`를 사용한다는 차이가 있습니다.

## 케이스 클래스

Scala의 패턴 매칭은 `케이스 클래스(Case Classes)`와 함께 사용하면 더욱 강력한 기능을 제공합니다.

```scala
sealed trait Notification

case class Email(sender: String, title: String, body: String) extends Notification
case class SMS(caller: String, message: String) extends Notification
case class VoiceRecording(contactName: String, link: String) extends Notification
```


이번 예제에서는 알림(Notification)의 형태에 따라 각기 다른 메시지를 전달하는 기능을 구현해보겠습니다.
우선 `Notification`이라는 trait을 정의하고 이를 상속하는 `Email`, `SMS`, `VoiceRecording`이라는 케이스 클래스들을 정의합니다.

> `trait`은 Java의 `Interface`와, `case class`는 Java의 `Recode`와 유사한 개념입니다.

```scala
def showNotification(notification: Notification): String = {
  notification match {
    case Email(sender, title, _) =>
      s"You got an email from $sender with title: $title"
    case SMS(number, message) =>
      s"You got an SMS from $number! Message: $message"
    case VoiceRecording(name, link) =>
      s"you received a Voice Recording from $name! Click the link to hear it: $link"
  }
}
```

`showNotification`이라는 메서드를 선언합니다. 해당 메서드는 `Notification`을 매개변수를 받으며 구체적인 타입에 따라 다른 문자열을 생성합니다. 이 경우의 `언더스코어(_)`는 불필요한 값을 무시하는 문법입니다. Email의 경우 body를 사용하지 않기 때문에 언더스코어를 사용하여 무시하였습니다.

```scala
val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms)) // prints You got an SMS from 12345! Message: Are you there?
println(showNotification(someVoiceRecording)) // prints You received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

이제 앞서 선언한 메서드를 호출해 보겠습니다. `SMS`와 `VoiceRecording` 각각 매개변수로 전달하였더니, 서로 다른 메시지가 출력되는 것을 볼 수 있습니다.

# Project Amber

[Project Amber](https://openjdk.org/projects/amber/)는 간결하지만 생산적인 자바 언어를 만들기 위한 프로젝트입니다. 널리 알려진 `JEP(JDK Enhancement Proposal)`에는 [JEP 395: Records](https://openjdk.org/jeps/395)와 [JEP 409: Sealed Classes](https://openjdk.org/jeps/409)가 있습니다. 이번 챕터에서는 Java 14에서 21까지 적용된 주요 `Pattern Matching` 관련 JEP들을 살펴보도록 하겠습니다.

> JEP는 JDK 개선을 위해 오라클이 초안을 작성하는 프로세스이며, 이러한 JEP들은 프로젝트라는 단위로 묶여서 관리됩니다.

## Switch Expressions

> [JEP 361: Switch Expressions](https://openjdk.java.net/jeps/361)

`Switch 식(Switch Expressions)`은 `Java 14`에서 도입되었으며, `Switch 문(Switch Statements)`의 개선 버전입니다. 간혹 국내 블로그 등지에서 Switch Expressions를 개선된 스위치문으로 소개하는 경우가 있습니다. 하지만 명확하게 이는 틀린 명칭입니다. 프로그램의 동작만 정의하는 `Statements(문)`과 값을 반환하는 `Expressions(식)`은 서로 다른 개념이기 때문입니다.

### AS-IS

`Switch 문`은 특정 변수의 비교하여 다양한 조건을 처리하는 조건 제어문입니다. 사용의 편리함 때문에 많은 개발자들이 활용하는 구문이지만, 몇 가지 단점이 존재합니다.

```java
for (NodeFlag flag : source) {
  switch (flag) {
    case NOFLAGS:
      flags.add(Flag.NOFLAGS);
      break;
    case EVENTUAL_FAIL:
      flags.add(Flag.PFAIL);
      break;
    case FAIL:
      flags.add(Flag.FAIL);
      break;
    case HANDSHAKE:
      flags.add(Flag.HANDSHAKE);
      break;
    case MASTER:
      flags.add(Flag.MASTER);
      break;
    case MYSELF:
      flags.add(Flag.MYSELF);
      break;
    case NOADDR:
      flags.add(Flag.NOADDR);
      break;
    case SLAVE:
    case REPLICA:
      flags.add(Flag.REPLICA);
      break;
  }
}
```

위 예시 코드는 `Switch 문`이 가지고 있는 문제점이 잘 드러나 있습니다. 조건의 개수에 비해 지나치게 코드가 비대해집니다. 또한 Switch 문은 `break` 키워드를 사용하지 않으면 아래 브랜치의 로직까지 수행하는 `fail through`라는 특징이 있습니다. 이러한 특징 때문에 break를 빼먹는 실수를 하게 되면 논리적인 오류가 발생할 수 있습니다.

### TO-BE

`Switch 식`은 앞서 설명한 `Switch 문의 단점`들을 모두 해결해줍니다. 

```java
for (NodeFlag flag : source) {
  switch (flag) {
    case NOFLAGS -> flags.add(Flag.NOFLAGS);
    case EVENTUAL_FAIL -> flags.add(Flag.PFAIL);
    case FAIL -> flags.add(Flag.FAIL);
    case HANDSHAKE -> flags.add(Flag.HANDSHAKE);
    case MASTER -> flags.add(Flag.MASTER);
    case MYSELF -> flags.add(Flag.MYSELF);
    case NOADDR -> flags.add(Flag.NOADDR);
    case SLAVE, REPLICA -> flags.add(Flag.REPLICA);
  }
}
```

Switch 식은 람다식과 유사한 `->`를 사용하여 코드를 명시적이면서 간결하게 표현할 수 있습니다. 게다가 `fail through`라는 특징이 없으므로 논리적인 오류가 발생할 위험성이 줄어듭니다.

### Switch 문 vs Switch 식

```java
switch (returnType) {
  case BOOLEAN:
    return ScriptOutputType.BOOLEAN;
  case MULTI:
    return ScriptOutputType.MULTI;
  case VALUE:
    return ScriptOutputType.VALUE;
  case INTEGER:
    return ScriptOutputType.INTEGER;
  case STATUS:
    return ScriptOutputType.STATUS;
  default:
    throw new IllegalArgumentException("Return type " + returnType + " is not a supported script output type");
}
```

위 예시 코드는 앞서 살펴본 코드와 살짝은 다른 케이스입니다. `case 브랜치` 내부에서 `return문을 사용`하여 값을 반환합니다. 그렇기 때문에 break를 사용할 필요가 없습니다. 이러한 경우는 `Switch 식`을 통해 어떻게 개선할 수 있을까요?

```java
return switch (returnType) {
  case BOOLEAN -> ScriptOutputType.BOOLEAN;
  case MULTI -> ScriptOutputType.MULTI;
  case VALUE -> ScriptOutputType.VALUE;
  case INTEGER -> ScriptOutputType.INTEGER;
  case STATUS -> ScriptOutputType.STATUS;
  default ->
    throw new IllegalArgumentException("Return type " + returnType + " is not a supported script output type");
};
```

앞서 `Switch 식`은 값을 반환하는 특징을 가진다고 설명해드렸습니다. 이러한 특징을 이용한다면 `switch` 키워드 그 자체를 반환하는 것이 가능합니다. 이를 통해 기존 코드의 `반복되는 return문을 제거`할 수 있습니다.

> [이번 챕터](#switch-expressions)에서 사용된 예시들은 `Spring Data Redis`의 코드를 개선한 사례에서 발췌하였습니다.
> 자세한 내용이 굼금하시다면 제가 올린 PR인 [Spring Data Redis #2706](https://github.com/spring-projects/spring-data-redis/pull/2706)을 참고해주세요.

## Pattern Matching for instanceof

> [JEP 394: Pattern Matching for instanceof](https://openjdk.java.net/jeps/394)

이번에 살펴볼 기능은 Java 16에서 추가되었습니다. 자바의 클래스 타입을 비교하는 `instanceof`를 패턴 매칭으로 사용할 수 있도록 해주는 기능입니다.

### AS-IS

```java
if (obj instanceof String) {
  String s = (String)obj;
  ... use s ...
}
```

혹시 Java 16 이전 버전을 사용하고 계신가요? 그렇다면 다음과 같이 if문으로 타입을 확인하였더라도 내부에서 `형변환을 수행`해주어야 합니다. 하지만 이미 타입을 확인하였는데 형변환을 해주어야 된다니 무언가 어색하다는 생각이 듭니다.

### TO-BE

```
if (obj instanceof String s) {
  ... use s ...
}
```

그래서 Java 16에서는 이러한 불필요한 형변환 작업을 하지 않더라도 `타입이 매칭`되도록 개선하였습니다. 그래서 이미 타입을 instanceof로 확인하였다면 바로 특정 타입의 변수로 사용할 수 있습니다.

## Pattern Matching for switch

> [JEP 441: Pattern Matching for switch](https://openjdk.java.net/jeps/441)

여기서부터는 드디어 이번 Java 21에 추가된 기능을 소개해드리겠습니다. 스위치를 위한 패턴 매칭은 Java 14에서 추가된 `Switch 식`과 Java 16에서 추가된 `Pattern Matching for instanceof`를 결합한 기능입니다.

### AS-IS

```java
static String formatter(Object obj) {
    String formatted = "unknown";
    if (obj instanceof Integer i) {
        formatted = String.format("int %d", i);
    } else if (obj instanceof Long l) {
        formatted = String.format("long %d", l);
    } else if (obj instanceof Double d) {
        formatted = String.format("double %f", d);
    } else if (obj instanceof String s) {
        formatted = String.format("String %s", s);
    }
    return formatted;
}
```

앞서 살펴본 [Pattern Matching for instanceof](#pattern-matching-for-instanceof)에서는 instanceof를 통해 타입을 비교하였습니다. 하지만 해당 기능은 if문과 함께 사용되어야만 했습니다.

### TO-BE

```java
static String formatterPatternSwitch(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
    };
}
```

하지만 이제는 if문이 아닌 switch 식에서도 타입을 비교할 수 있게 되었습니다. 이를 통해 코드가 더욱 간결해지고 가독성이 좋아졌습니다. 또한 형변환 없이 바로 특정 타입의 변수로 사용할 수 있습니다.

## Record Patterns

> [JEP 440: Record Patterns](https://openjdk.java.net/jeps/440)

`레코드(Record)`는 Java에서 사용되는 불변 객체 타입입니다. `Record Patterns`은 `구조분해 할당(deconstruct)`을 통해 레코드 내에 존재하는 변수들을 쉽게 사용할 수 있도록 제공합니다.

### AS-IS

```java
record Point(int x, int y) {}

static void printSum(Object obj) {
    if (obj instanceof Point p) {
        int x = p.x();
        int y = p.y();
        System.out.println(x+y);
    }
}
```

이전까지 레코드는 반드시 `getter`를 통해 내부의 변수에 접근할 수 있었습니다. 그래서 위 예시에서 필요한 값은 x와 y이지만  point 타입의 변수를 통해 값을 가져와야 했습니다.

### TO-BE

```java
static void printSum(Object obj) {
    if (obj instanceof Point(int x, int y)) {
        System.out.println(x+y);
    }
}
```

하지만 이제는 그럴 필요가 없습니다. `Record Patterns`을 이용한다면 레코드 내의 필요한 값만 가져올 수 있습니다. 이를 이용한다면 간결하고 가독성이 좋은 코드를 작성할 수 있습니다.

# Pattern Matching with Java 21

여기까지 `Project Amber`를 통해 Java에 추가된 패턴 매칭 기능들을 살펴보았습니다. 이번 챕터에서는 이 모든 기능을 함께 활용하는 방법을 소개해드리고자 합니다.

```java
sealed interface Notification {}

record Email(String sender, String title, String body) implements Notification {}
record SMS(String caller, String message) implements Notification {}
record VoiceRecording(String contactName, String link) implements Notification {}
```

앞서 블로그 초반에 소개해드린 [Pattern Matching with Scala](#케이스-클래스) 챕터의 예시를 살펴보겠습니다. Scala로 작성된 코드를 Java로 작성한다면 위와 같이 작성할 수 있을 것입니다. `Notification`을 `sealed interface`로 정의하고 세부적인 데이터 구조를 `record`로 정의합니다. 

```java
public static String showNotification(Notification notification) {
    return switch (notification) {
        case Email(String sender, String title, String body) ->
            String.format("You got an email from %s with title: %s", sender, title);
        case SMS(String caller, String message) ->
            String.format("You got an SMS from %s! Message: %s", caller, message);
        case VoiceRecording(String contactName, String link) ->
            String.format("You received a Voice Recording from %s! Click the link to hear it: %s", contactName, link);
    };
}
```

이제 `showNotification` 메서드도 작성해주겠습니다. Java 21에서 [Pattern Matching for switch](#pattern-matching-for-switch)을 적용된 덕분에 Notification의 `타입을 비교`하고, 필요한 로직을 수행할 수 있습니다. 또한 Notification의 하위 타입들은 모두 Record이므로 [Record Patterns](#record-patterns)를 적용할 수 있으므로, `구조분해 할당`을 통해 필요한 멤버 변수만 가져올 수 있습니다.

```java
var someSms = new SMS("12345", "Are you there?");
var someVoiceRecording = new VoiceRecording("Tom", "voicerecording.org/id/123");

System.out.println(showNotification(someSms)); // prints You got an SMS from 12345! Message: Are you there?
System.out.println(showNotification(someVoiceRecording)); // prints You received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

최종적으로 완성된 코드를 실행하면 앞서 Scala에서 실행한 것과 동일한 결과를 얻으실 수 있습니다.

# 프리뷰 기능까지 적용하기

하지만 여전히 레코드 패턴 내 불필요한 인자를 무시할 수 없다는 점과 문자열 생성을 위해 `String.format`을 사용하는 방법은 아쉬움이 남습니다. 이러한 것을 보완한 기능들은 이번 Java 21의 `프리뷰 기능`으로 릴리즈 되었습니다.

관련 JEP는 다음과 같습니다.

- [JEP 430: String Templates](https://openjdk.org/jeps/430)
- [JEP 443: Unnamed Patterns and Variables](https://openjdk.org/jeps/443)

```java
public static String showNotification(Notification notification) {
    return switch (notification) {
        case Email(String sender, String title, _) ->
            STR."You got an email from \{sender} with title: \{title}"; // String interpolation
        case SMS(String caller, String message) ->
            STR."You got an SMS from \{caller}! Message: \{message}";
        case VoiceRecording(String contactName, String link) ->
            STR."You received a Voice Recording from \{contactName}! Click the link to hear it: \{link}";
    };
}
```

이러한 기능들이 모두 적용된 자바 예제는 위와 같습니다. `STR` 키워드는 `String Templates`를 사용하기 위한 키워드입니다. 이를 통해 문자열 내부에 변수를 삽입할 수 있습니다. 또한 `record`의 멤버 변수 중 불필요한 인자는 `_`를 통해 무시할 수 있습니다.

최종적으로 프리뷰 기능까지 적용한 코드를 보면 Scala의 코드와 매우 유사하다는 사실을 깨달을 수 있습니다. 이는 `Project Amber`가 추구하는 방향성이 Scala와 같은 함수형의 장점을 Java에 적용하는 것이라는 사실을 보여줍니다.

# 마치며

Java는 8 버전에 `Lambda Expression(람다식)`을 추가한 이후 함수형 프로그래밍 언어의 특징을 계속해서 적용해오고 있습니다. 그리고 이번 Java 21에서는 `Pattern Matching(패턴 매칭)`의 주요 기능들을 다수 적용하면서 함수형 패러다임을 적극적으로 수용하는 모습을 보여주었습니다.

이번 릴리즈를 통해 Java가 낡은 언어가 아닌 `현대적인 언어`로 탈바꿈하고 있다는 것을 느낄 수 있었습니다. 모던 Java 시대에서 다른 JVM 언어들은 또 어떤 강점을 보여줄 수 있을 지 기대가 됩니다. 또한 `가상 스레드(Virtual Thread)`의 도입으로 인한 JVM 내 동시성 프로그래밍의 패러다임 변화는 Kotlin의 Coroutine을 대체할 수 있을 것인지도 하나의 관전 포인트입니다.

동시에 특정 언어만 고집하는 것은 좋지 않다는 생각이 다시 한 번 들었습니다. 저는 Java가 메인 언어임에도 Scala나 Python에 관심을 가지고 공부해왔습니다. 그 덕분에 이번 Java 21의 패턴 매칭을 빠르게 이해할 수 있었으며, 이번 블로그 포스팅도 작성하는 계기가 되었습니다. 점차 언어간의 문법의 경계가 사라지는 가운데, 여러분들도 다양한 언어를 경험해보고 다양한 패러다임을 경험해보는 것은 어떨까요?
