---
title: "Java 21 Pattern Matching Usecase With Scala"
translationKey: posts/language/java/java-21-pattern-matching-usecase-with-scala
date: 2023-09-21T22:27:53+09:00
series:
  - Java
categories:
  - Java
tags:
  - Java
draft: false
summary: >
  The highly developed Java is indistinguishable from Scala.
---


On September 19, 2023, the long-awaited Java 21 was released. While most people's attention is focused on `Virtual Thread`, `Pattern Matching` caught my eye. This is because I am currently working as a mentee for the `ZIO` team in `Open Source Contribution Academy` and have experience using the function frequently.

> ZIO is an asynchronous concurrency library in Scala.

So, in this post, I would like to learn what `pattern matching` is through an example conducted in ZIO Study and introduce `Project Amber`, a project to implement pattern matching in Java. Finally, we will implement the same example as in Scala using Java 21's pattern matching.

## Pattern Matching with Scala

`Pattern Matching` is a function that checks patterns for expressions. In this chapter, we will briefly learn what pattern matching is with an example of [Tour of scala](https://docs.scala-lang.org/tour/pattern-matching.html).

### Basic grammar

Scala provides the following keywords for pattern matching.

- `match`: Keyword that starts pattern matching
- `case`: Keyword that defines a branch

```scala
import scala.util.Random

val x: Int = Random.nextInt(10)

x match
  case 0 => "zero"
  case 1 => "one"
  case 2 => "two"
  case _ => "other"
```

The above example declares a variable x containing `random value` from 0 to 9, and branches depending on the value of x.
It is similar to Java's switch statement, with the difference being that it uses `underscore(_)` instead of the `default` keyword.

### Case class

Scala's pattern matching is even more powerful when used with `Case Classes`.

```scala
sealed trait Notification

case class Email(sender: String, title: String, body: String) extends Notification
case class SMS(caller: String, message: String) extends Notification
case class VoiceRecording(contactName: String, link: String) extends Notification
```


In this example, we will implement a function that delivers different messages depending on the type of notification.
First, define a trait called `Notification` and define case classes called `Email`, `SMS`, and `VoiceRecording` that inherit it.

> `trait` is a similar concept to Java's `Interface`, and `case class` is similar to Java's `Recode`.

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

Declare a method called `showNotification`. The method takes `Notification` as a parameter and creates different strings depending on the specific type. `Undercore(_)` in this case is a grammar that ignores unnecessary values. In the case of Email, since the body is not used, an underscore was used to ignore it.

```scala
val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms)) // prints You got an SMS from 12345! Message: Are you there?
println(showNotification(someVoiceRecording)) // prints You received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

Now let's call the method declared earlier. When `SMS` and `VoiceRecording` are each passed as parameters, you can see that different messages are output.

## Project Amber

[Project Amber](https://openjdk.org/projects/amber/) is a project to create a concise yet productive Java language. The popular `JEP(JDK Enhancement Proposal)` include [JEP 395: Records](https://openjdk.org/jeps/395) and [JEP 409: Sealed Classes](https://openjdk.org/jeps/409). In this chapter, we will look at the major `Pattern Matching`-related JEPs applied from Java 14 to Java 21.

> JEP is a process drafted by Oracle to improve the JDK, and these JEPs are grouped and managed in units called projects.

### Switch Expressions

> [JEP 361: Switch Expressions](https://openjdk.java.net/jeps/361)

`Switch Expressions` was introduced in `Java 14` and is an improved version of `Switch Statements`. Sometimes, Switch Expressions are introduced as improved switch statements on Korean blogs. But clearly this is a misnomer. This is because `Statements`, which only defines the behavior of the program, and `Expressions`, which returns a value, are different concepts.

#### AS-IS

`Switch statement` is a conditional control statement that processes various conditions by comparing specific variables. This syntax is used by many developers because of its ease of use, but it has some drawbacks.

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

The example code above clearly shows the problem with `Switch statement`. The code becomes too bloated compared to the number of conditions. Additionally, the Switch statement has a feature called `fail through` that even performs the logic of the branch below if the `break` keyword is not used. Because of this characteristic, if you make the mistake of omitting break, a logical error may occur.

#### TO-BE

`Switch expression` solves all `Disadvantages of Switch statement` described above.

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

Switch expressions use `->`, similar to lambda expressions, to express your code explicitly and concisely. Moreover, since there is no feature called `fail through`, the risk of logical errors is reduced.

#### Switch statement vs Switch expression

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

The example code above is slightly different from the code we looked at earlier. Inside `case branch`, we return the value by `return statement`. That's why you don't need to use break. How can these cases be improved through `Switch expression`?

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

We explained earlier that `Switch expression` has the characteristic of returning a value. Using this feature, it is possible to return the `switch` keyword itself. This allows you to `remove repetitive return statements` your existing code.

> The examples used in [this chapter](#switch-expressions) are excerpted from cases where the code of `Spring Data Redis` was improved.
> If you are curious about the details, please refer to the PR I posted, [Spring Data Redis #2706](https://github.com/spring-projects/spring-data-redis/pull/2706).

### Pattern Matching for instanceof

> [JEP 394: Pattern Matching for instanceof](https://openjdk.java.net/jeps/394)

The features we will look at this time were added in Java 16. This function allows you to use `instanceof`, which compares Java class types, for pattern matching.

#### AS-IS

```java
if (obj instanceof String) {
  String s = (String)obj;
  ... use s ...
}
```

Are you using a version prior to Java 16? In that case, even if the type is checked with an if statement as follows, `perform type conversion` must be done internally. However, I think it's a bit awkward to have to convert the type even though I've already checked it.

#### TO-BE

```
if (obj instanceof String s) {
  ... use s ...
}
```

So in Java 16, we improved it to `type matching` even without such unnecessary type conversion work. So, if you have already confirmed the type with instanceof, you can immediately use it as a variable of a specific type.

### Pattern Matching for switch

> [JEP 441: Pattern Matching for switch](https://openjdk.java.net/jeps/441)

From here, we will finally introduce the features added to Java 21. Pattern matching for switches is a combination of `Switch expression` added in Java 14 and `Pattern Matching for instanceof` added in Java 16.

#### AS-IS

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

In [Pattern Matching for instanceof](#pattern-matching-for-instanceof), which we looked at earlier, types were compared using instanceof. But that function had to be used with an if statement.

#### TO-BE

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

However, now it is possible to compare types in switch expressions rather than if statements. This makes the code more concise and more readable. Additionally, it can be used directly as a variable of a specific type without type conversion.

### Record Patterns

> [JEP 440: Record Patterns](https://openjdk.java.net/jeps/440)

`Record` is an immutable object type used in Java. `Record Patterns` provides easy use of variables that exist in records through `deconstruct`.

#### AS-IS

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

Previously, records could only access internal variables through `getter`. So, in the example above, the required values ​​are x and y, but the values ​​had to be retrieved through a variable of type point.

#### TO-BE

```java
static void printSum(Object obj) {
    if (obj instanceof Point(int x, int y)) {
        System.out.println(x+y);
    }
}
```

But now you don't have to. If you use `Record Patterns`, you can get only the necessary values ​​in the record. Using this, you can write concise and readable code.

## Pattern Matching with Java 21

Up to this point, we have looked at the pattern matching functions added to Java through `Project Amber`. In this chapter, I would like to introduce how to use all of these features together.

```java
sealed interface Notification {}

record Email(String sender, String title, String body) implements Notification {}
record SMS(String caller, String message) implements Notification {}
record VoiceRecording(String contactName, String link) implements Notification {}
```

Let’s look at the example of the [Pattern Matching with Scala](#Case-Class) chapter introduced at the beginning of the blog. If you write code written in Scala in Java, you can write it like the above. Define `Notification` as `sealed interface` and the detailed data structure as `record`.

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

Now we will also write the `showNotification` method. Thanks to the application of [Pattern Matching for switch](#pattern-matching-for-switch) in Java 21, you can perform `Compare types` of Notification and perform the necessary logic. Additionally, since all subtypes of Notification are Record, [Record Patterns](#record-patterns) can be applied, so only necessary member variables can be retrieved through `Destructuring Assignment`.

```java
var someSms = new SMS("12345", "Are you there?");
var someVoiceRecording = new VoiceRecording("Tom", "voicerecording.org/id/123");

System.out.println(showNotification(someSms)); // prints You got an SMS from 12345! Message: Are you there?
System.out.println(showNotification(someVoiceRecording)); // prints You received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

When you finally run the completed code, you will get the same results as if you ran it previously in Scala.

## Apply the preview function as well

However, it is still disappointing that unnecessary arguments in the record pattern cannot be ignored and that `String.format` is used to create strings. The features that complement these were released as `Preview Features` of Java 21.

The relevant JEPs are:

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

A Java example with all these functions applied is shown above. The `STR` keyword is a keyword for using `String Templates`. This allows you to insert variables inside a string. Additionally, unnecessary arguments among member variables of `record` can be ignored through `_`.

If you look at the code with the final preview function applied, you can realize that it is very similar to Scala code. This shows that the direction pursued by `Project Amber` is to apply the advantages of functional types such as Scala to Java.

## Conclusion

Java has continued to adopt features of a functional programming language since adding `Lambda Expression` in version 8. And in this year's Java 21, many of the key features of `Pattern Matching` were applied, showing an active acceptance of the functional paradigm.

With this release, we could feel that Java is transforming from an old language into a `modern language`. I look forward to seeing what other strengths other JVM languages ​​​​can show in the modern Java era. Also, one point to watch is whether the paradigm shift in concurrent programming within the JVM due to the introduction of `Virtual Thread` will be able to replace Kotlin's Coroutine.

At the same time, it occurred to me once again that it is not a good idea to only stick to a specific language. Even though Java is my main language, I have been interested in studying Scala and Python. Thanks to this, I was able to quickly understand pattern matching in Java 21, and it served as an opportunity to write this blog post. As the grammatical boundaries between languages ​​are gradually disappearing, why not try out different languages ​​and experience different paradigms?
