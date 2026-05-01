---
title: "Why Spring"
date: 2022-07-21T22:47:38+09:00
draft: false
series:
  - Spring
categories:
  - Spring
tags:
  - Java
  - Spring
  - J2EE
  - EJB
---

> Note: This English page scaffold was auto-generated. Full manual translation will follow.


Spring is the `Application Framework` for enterprise environments and is the standard for Java-based web application development today. To fully understand Spring, you need to know its `background of appearance`.

This is because Spring is the result of solving the problems of `J2EE(Java 2 Platform, Enterprise Edition)` for enterprise application development. Understanding the background of spring's features and advantages will enhance your understanding of spring.

## J2EE

`J2EE(Java 2 Platform, Enterprise Edition)` is a set of specifications for enterprise application development. It was created by extending `J2SE(Java 2 Platform, Standard Edition)`, a commonly used Java language.

> 💡 `J2EE` and `J2SE` are called differently depending on the version. When version 5 was released, they were renamed to `Java EE` and `Java SE`. In the case of `Java EE`, it was changed once more to `Jakarta EE` when Oracle transferred its operating rights to the Eclipse Foundation.

### Specifications

- J2SE APIs
- Servlet
- JSP
- EJB
- Focus on core requirements...

J2EE is basically based on J2SE as mentioned above, and adds elements necessary for enterprise application development. Representative examples include `Servlet`, `JSP`, and `EJB`. If you have ever encountered Java-based web programming, you may have heard of it.

### Application Server

`J2EE` requires `J2EE Application Server` that satisfies its specifications. This was commonly referred to as `WAS(Web Application Server)`. Representative WAS includes `JEUS`, `Weblogic`, and `WebSphere`.

> 💡 Nowadays, you can build backend servers not only in Java but also in other languages ​​such as `Node.js` or `Flask`, so you can no longer say `WAS == J2EE Applcation Server`.

### Problem

Crucially, there is a `three` reason for the paradigm change from J2EE to Spring.

#### Operating costs

First of all, J2EE had very high operating environment requirements. Building a full-stack server that meets all J2EE specifications was extremely difficult and the operating costs were astronomical.

#### Platform dependencies

The goal of J2EE was to leverage Java's platform-independent nature to provide `environment-independent middleware`. However, contrary to these purposes, the actual J2EE application was `platform dependent`.

The reason was that there were many features that were fundamentally not defined in J2EE. This ultimately resulted in different implementation methods for each WAS manufacturer, and attempts to break away from dependency on the operating environment failed.

#### Winter called EJB

`EJB(Enterprise Java Bean)` is a distributed component technology that processes business logic in a J2EE environment. The intention behind this was good, but it was too complex to use. `Rod Johnson`, the developer of `Spring Framework`, said the following in the book `J2EE Development without EJB`:

*"To avoid making a failure like EJB, the first thing to think about is the basic core values, which are composed of six things."*

```plaintext
- Simplicity
 - Not a complex architecture like EJB that cannot scale down.
 - Prefer a simple architecture that can scale up through refactoring when needed.
- Productivity
 - In practice, productivity is what matters most, because it reduces cost and time.
- OO
 - Java is already an excellent OO language. Why should specific technologies like EJB come first and hinder good OO design?
- Primacy of Requirement
 - Focus on core requirements.
- Empirical Process
 - Focus on core requirements.
 - The IT industry often has emotional trends and unverified rumors; use technologies and structures that you validate yourself.
- Testability
 - TDD and TFD (Test First Development) are trending.
 - Focus on core requirements.
```

## Spring

Spring originated when Rod Johnson, developer of the Spring Framework, pointed out problems with existing J2EE through a book called `J2EE Design and Development`. We demonstrate through 30,000 lines of example code that it is possible to develop high-quality, scalable applications without EJB.

After the book was published, Juergen Hoeller and Yann Caroff proposed `open source project`, which became the current Spring framework. The name Spring was suggested by Yann Caroff and means ‘J2EE, a new beginning after the winter’.

*"the fact that Spring represented a fresh start after the “winter” of traditional J2EE"*

### characteristic

#### Built-in WAS

Spring does not require WAS to implement all of the J2EE specifications. In other words, WAS with only `Servlet Container` technology implemented works well. By default, Spring includes `Tomcat` or `Jetty`, so there is no need to install a separate WAS.

#### POJO

`POJO(Plain Old Java Object)` refers to a regular Java object. It was first created by `Martin Fowler` as a word that emerged as a backlash against using objects dependent on frameworks such as EJB.

POJOs are the core of Spring for solving framework dependency issues. Because it is a POJO-based framework, Spring allows you to write object-oriented, concise code and make testing easy.

### Relationship to J2EE

The most confusing part while studying Spring was its relationship with J2EE. Depending on the search results, Spring is sometimes described as a `replacement to J2EE` technology, but it is also introduced as a `supporting J2EE development` framework. Two completely incompatible explanations add to the confusion. Which explanation is correct?

To conclude, `Spring is a technology that replaces J2EE` is correct. However, it is difficult to say that it has nothing to do with J2EE. The reason is because springs were made with `adopting some of the J2EE technologies`. Typically, `servlet` is still used to handle the communication part in Spring. Although Spring does not fully satisfy the J2EE specifications, it can be said that it does `satisfy some specifications`.

Personally, I think this misunderstanding started with the title of the book `J2EE Development without EJB` by `Rod Johnson`, who developed Spring. `J2EE development without EJB` makes me feel like Spring helps J2EE development. However, it appears that he actually wanted to talk about the emergence of a new framework.

## Conclusion

As I was organizing things I was curious about while using Spring, I came to organize the background of how Spring appeared. Through this, I was able to spend time studying technologies that existed before spring, which I only knew at first glance, and was able to better understand the reasons and advantages of using spring.

There are still many things left that I couldn't mention here. Among Spring's features, only the built-in WAS and POJO were mentioned for the sake of `Focus on features that conflict with the problems of J2EE`. I will organize other features of Spring and POJOs in detail in future posts.

## References

- [Jakarta EE - Wikipedia](https://en.wikipedia.org/wiki/Jakarta_EE)
- [History of Java EE and relationship with Spring - okky](https://okky.kr/article/415474)
- [When should I use Spring Boot? (2) Is external WAS absolutely necessary? - zepinos](https://zepinos.tistory.com/86)
- [Why are we talking about J2EE development without EJB? - Toby](https://cafe.naver.com/sylee999/85)
- [Spring Framework: The Origins of a Project and a Name - Spring Blog](https://spring.io/blog/2006/11/09/spring-framework-the-origins-of-a-project-and-a-name)