---
title: "Primitive Type And Reference Type"
date: 2021-07-16T14:06:26Z
series:
  - Java
categories:
  - Java
tags:
  - Java
  - General
draft: false
summary: >
  Let’s learn about Java’s basic data types and reference data types!
---

Data type type
---

![java data types](http://www.btechsmartclass.com/java/java_images/java-data-types.jpg#center)

Data types are largely divided into `basic data types` and `reference data types`. In this post, we will only look at the basic data types in Java programming. We will cover the details in a later post.

### Primitive Type

`Basic type` is the same data type as `int`, `float`, `char`, and `boolean` that we commonly know and use. Since Java is a language written with static typing, you must be familiar with basic data types. Placing data types that fit the purpose of use in the right place allows you to write efficient code.

| data type | meaning | memory size | range | default |
|--------|------|--------------|------|--------|
| byte | 8-bit integer | 1 byte | -2⁷ ~ 2⁷-1 | 0 |
| short | 16-bit integer | 2 bytes | -2¹⁵ ~ 2¹⁵-1 |0 |
| int | 32-bit integer | 4 bytes | -2³¹ ~ 2³¹-1 | 0 |
| long | 64-bit integer | 8 bytes | -2⁶³ ~ 2⁶³-1 | 0L |
| float | 32-bit floating point (IEEE 754) | 4 bytes | -3.40E+38 ~ 3.40E+38 | 0.0f |
| double | 64-bit floating point (IEEE 754) | 8 bytes | 1.79E+308 ~ 1.79E+308 | 0.0d |
| char | 16-bit Unicode characters | 2 bytes | 0 to 2¹⁶-1 | \u0000 |
| boolean | logical type | 1 bit | 0 or 1 | 0 (false) |

> Unlike C/C++, Java does not have an unsigned type among integer types. Strictly speaking, from Java 8 onwards, several static methods can be used through `int`, `long` types and, in some cases, `Wrapper Class`.

### Reference Type `Reference Type` is a data type created based on `Basic Data Type`. Representative examples include `Class`, `Interface`, and `Enum` such as String, Array, Map, and Set provided by Java. Additionally, users can define reference types as needed.

#### Object

All `Class` and `Enum` inherit the `Object` class. In other words, Object is a generalized type of all Classes and Enums. The point to note here is that Interface does not inherit Object. Information related to this is clearly revealed in the Java API document [Tree](https://docs.oracle.com/en/java/javase/16/docs/api/overview-tree.html).

> [Java API Documentation](https://docs.oracle.com/en/java/javase/16/docs/api/index.html) defines detailed specifications such as all provided `Class`, `Interface`, and `Enum`. This site is very helpful in understanding Java in depth, so it is a good idea to visit it often. However, you must be careful because the API implementation differs depending on the Java version.

#### String

It is well known that `String` is not a basic data type. String is a reference data type implemented as an array of `char`.

> In the case of `C/C++`, in order to use a string, a char type array is actually used directly.

The reason why Java provides the `String` type separately, unlike `C/C++`, is to provide `useful methods` in strings. You can easily manipulate strings using methods such as charAt, concat, equals, indexOf, and split.

#### Array

`Array` is also not a basic data type. This can be tested with the following simple code:

```java
int[] array = new int[10];
System.out.println(array instanceof Object);
// true
```

Since all classes are inheritance classes of Object, you can see from the code above that the array is not a basic type. If the array was a basic type, the result of executing the above code should be `false`.

#### Wrapper class`Wrapper Class` is a class that wraps the basic data type. Representative examples include Byte, Short, Integer, Long, Float, Double, Character, and Boolean. The reason for using this is the same as the reason for using `String` explained earlier. The advantage of wrapping a basic data type with `class` is that you can `provide useful methods`.

But a more important reason lies in `Generic`. The parameter `T` used in generics can only accept `Object` data type. This means that only objects defined as classes are received. However, when writing code, there are times when generics need to be applied to basic data types. In this case, using `Wrapper Class` solves the problem.

> It would be easier to understand if you think that `Wrapper Class` is provided to enable `Flexible Programming` through generics to be applied to basic data types.

### Differences

Previously, we learned what basic data types and reference data types are, and what the differences are. I would like to conclude this post by briefly looking at the differences between them.

- Reference data types can have methods, unlike basic data types.
- The default value of the reference data type is `null`.
 - Therefore, if a reference object is not initialized, `nullPointerException` occurs.
- Refer to [Table](#primitive-type) above for the default values ​​​​of basic data types.

References
---

- [btechsmartclass - java data types](http://www.btechsmartclass.com/java/java-data-types.html)
- [oracle - java documentation](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
