---
title: "Build Integration Test Environment"
date: 2023-08-30T14:03:48+09:00
draft: false
series:
  - Spring Data Meilisearch
categories:
  - Project
tags:
  - Java
  - Spring
  - Meilisearch
  - Library
summary: >
  Building an integrated test environment with Testcontainers and the JUnit 5 extension model
---

Integration testing is a very important part of the development process of [Spring Data Meilisearch](https://github.com/junghoon-vans/spring-data-meilisearch).
This is because the core of the project is whether it integrates with actual Meilisearch and operates properly.
In this post, I would like to introduce the method applied to test the actual project Core module called `MeilisearchTemplate`.

> In the Spring Data implementation, the `Template` class is a class that provides basic methods for accessing data storage.

## Testcontainers Meilisearch

The first thing we needed to do in our integration tests was to launch the instance that `Spring Data Meilisearch` would access.
There are many ways to run an instance, but `Testcontainers` was used to build a test environment that is independent of the development environment.

### Run container

The [Testcontainers Meilisearch](https://testcontainers.com/modules/meilisearch/) library allows you to easily build the testing environment needed for Meilisearch. `Testcontainers Meilisearch` is a library provided as a Testcontainers community module, and is a project that I am currently developing and maintaining.

```java
@ExtendWith(SpringExtension.class)
class MeilisearchTemplateTest {

 MeilisearchContainer meilisearch;

 @BeforeAll
 static void beforeAll() {
 meilisearch = new MeilisearchContainer().withMasterKey("masterKey");
 meilisearch.start();
 }

 // ...
}
```

The example code above sets up Meilisearch's master key and runs the container.

### Container connection settings

Now let's connect to the Meilisearch instance in our test code.
`Spring Data Meilisearch` is a method of connecting to a Meilisearch instance and supports both [JavaConfig](/posts/project/spring-data-meilisearch/support-configuration-with-annotation) and [XML Namespace](/posts/project/spring-data-meilisearch/support-configuration-with-namespace) configuration methods, but the settings required to build this test environment were created using the JavaConfig method.

```java
@Configuration
class Config extends MeilisearchConfiguration {
 @Override
 public ClientConfiguration clientConfiguration() {
 return ClientConfiguration.builder()
 .connectedTo("http://localhost:" + meilisearch.getMappedPort(7700))
 .withApiKey("masterKey").build();
 }
}
```

The code above uses the `connectedTo` method to specify the hostname and port of the Meilisearch instance.
The external port of a container executed through Testcontainers is bound to a random value each time.
Therefore, the container's port, such as `meilisearch.getMappedPort(7700)`, must be obtained dynamically at runtime.

### Full example code

Now let's look at the overall code. The code that tests the `MeilisearchTemplate` class has the following structure:> Omitted except for parts related to container settings.

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = { MeilisearchTemplateTest. Config.class })
class MeilisearchTemplateTest {

 MeilisearchContainer meilisearch;

 @BeforeAll
 static void beforeAll() {
 meilisearch = new MeilisearchContainer().withMasterKey("masterKey");
 meilisearch.start();
 }

 // ...

 @Configuration
 static class Config extends MeilisearchConfiguration {
 public ClientConfiguration clientConfiguration() {
 return ClientConfiguration.builder()
 .connectedTo("http://localhost:" + meilisearch.getMappedPort(7700))
 .withApiKey("masterKey").build();
 }
 }
}
```

Use the `@ContextConfiguration` annotation to specify the inner class `Config` as the setting.
This allows you to connect to and test containers launched through Testcontainers Meilisearch.

However, there are still problems with methods like this.
If there are many test files, the code to run the container must be written redundantly using the `@BeforeAll` annotation, and configuration files must be created redundantly.

## Aspect-oriented programming

A topic worth thinking about here is `Aspect Oriented Programming (AOP)`.

Aspect-oriented programming is a programming technique that increases modularity through `separation of concerns`.
Here, concerns are divided into `core` functions and `additional` functions.

![Aspect-Oriented Programming](https://cdn.codegym.cc/images/article/905659f3-ca60-47ed-847d-e3184c26b152/512.jpeg)

For example, if there is a function that different modules use in common, it is an additional function.
Features such as logging or security are also called `cross-cutting concerns` because they are used globally across multiple modules.

### Separation of concerns in test code

Let's recall the [MeilisearchTemplateTest](#full-example-code) code we looked at in the previous chapter.
The code was an integration test code to check whether MeilisearchTemplate functions properly.
If you look at the code from an AOP perspective, it can be divided as follows.

- `Core concern`: Perform test
- `Cross-cutting concerns`: Running and managing containers

What if container management, a cross-cutting concern, could be separated into modules?
In the test code, you will be able to write more modular code by focusing only on test execution.
You will also no longer need to duplicate container management logic across multiple test files.

## JUnit 5 extension model

When writing test code using JUnit 5, [Extension Model](https://junit.org/junit5/docs/snapshot/user-guide/#extensions) allows you to separate cross-cutting concerns into modules. The extension model allows you to extend and use the features provided by existing JUnit 5. The most representative extension model is the `SpringExtension` class provided by Spring Framework.
This class is responsible for creating and managing the test context of the Spring Framework, and was also implicitly used in the example code seen above.

In a similar way, you can create your own reusable extension models.

### Extension model implementation

In the example code we looked at earlier, the container was run using the `@BeforeAll` annotation.
Implementing this through the extension model is as follows:

```java
public class MeilisearchExtension implements BeforeAllCallback {

private final Lock initLock = new ReentrantLock();

 @Override
 public void beforeAll(ExtensionContext context) {
 initLock.lock();
 try {
 new MeilisearchConnection();
 } finally {
 initLock.unlock();
 }
 }
}
```

The `MeilisearchExtension` class implements the `BeforeAllCallback` interface, which allows the functionality defined in the `@BeforeAll` annotation to be implemented here.

> The `MeilisearchConnection` class is responsible for running and connecting containers. If you are curious about the full code, please refer to [repository](https://github.com/junghoon-vans/spring-data-meilisearch/tree/main/src/test/java/io/vanslog/spring/data/meilisearch/junit/jupiter).

### TestConfiguration implementation

Now let's implement the `MeilisearchTestConfiguration` class, which provides settings that retrieve the hostname and port of the launched container.

```java
@Configuration
public class MeilisearchTestConfiguration extends MeilisearchConfiguration {

 private static final String HTTP = "http://";

 private final MeilisearchConnectionInfo meilisearchConnectionInfo
 = MeilisearchConnection.meilisearchConnectionInfo();

 @Override
 public ClientConfiguration clientConfiguration() {
 return ClientConfiguration.builder()
 .connectedTo(HTTP + meilisearchConnectionInfo.getHost() + ":" + meilisearchConnectionInfo.getPort())
 .withApiKey(meilisearchConnectionInfo.getMasterKey()).build();
 }
}
```

The `MeilisearchTestConfiguration` class provides settings to connect to the Meilisearch instance by getting the host name and port of the container previously launched in the `MeilisearchExtension` class.

```java
@ExtendWith(SpringExtension.class)
@ExtendWith(MeilisearchExtension.class)
@ContextConfiguration(classes = { MeilisearchTestConfiguration.class })
class MeilisearchTemplateTest {
 // ...
}
```

Now you no longer need to write logic for creating and connecting a Meilisearch container for each test.
You can run the container through the `MeilisearchExtension` class and test by providing settings to connect to the container through the `MeilisearchTestConfiguration` class.

### Test annotation implementation

It's enough to proceed up to this point, but let's create an additional `@MeilisearchTest` annotation to write test code more concisely.

```java
@Retention(RetentionPolicy. RUNTIME)
@Target(ElementType. TYPE)
@ExtendWith(MeilisearchExtension.class)
@ExtendWith(SpringExtension.class)
@TestMethodOrder(MethodOrderer. OrderAnnotation.class)
public @interface MeilisearchTest {
}
```

The `@MeilisearchTest` annotation launches a container through the `MeilisearchExtension` class and creates a Spring context through the `SpringExtension` class.

```java
@MeilisearchTest
@ContextConfiguration(classes = { MeilisearchTestConfiguration.class })
class MeilisearchTemplateTest {
 // ...
}
```

Now you no longer need to use the `@ExtendWith` annotation, you can simply test using the `@MeilisearchTest` annotation.

## Conclusion

In this post, I introduced the methods used to build an integrated test environment.
It was a meaningful experience as it was an environment built with the `Testcontainers` library that I implemented myself and `extension model` of JUnit 5.

I thought a lot about how to write test code in a good way, and during this time, I was able to think about it in relation to `aspect-oriented programming`.

I am proud that [initial project goal](/posts/project/spring-data-meilisearch/introduction), which aims to create a Spring integration environment that is lacking in the Meilisearch ecosystem, is gradually being achieved.
I will continue adding features so that users can use the project more conveniently.

## References

- [Testcontainers Modules - Meilisearch](https://testcontainers.com/modules/meilisearch/)
- [What is AOP? Principles of aspect-oriented programming](https://codegym.cc/groups/posts/543-what-is-aop-principles-of-aspect-oriented-programming)
- [JUnit 5 Official Documentation - Extension Model](https://junit.org/junit5/docs/snapshot/user-guide/#extensions)
