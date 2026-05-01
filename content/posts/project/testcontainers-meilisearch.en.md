---
title: "Testcontainers Meilisearch"
date: 2023-07-21T22:47:52+09:00
draft: false
categories:
  - Project
tags:
  - Java
  - Testcontainers
  - Meilisearch
  - Library
summary: >
  We introduce Testcontainers Meilisearch and share what we felt while developing it.
---


## Project motivation

In the recent [Spring Data Meilisearch](/posts/project/spring-data-meilisearch/introduction) project, it was necessary to write test code to access the [Meilisearch](https://www.meilisearch.com) instance. We considered various methods, but ultimately decided that using `Testcontainers` was the most appropriate. However, Testcontainers did not officially support Meilisearch, and a class called `GenericContainer` had to be overridden. So, I decided to take this opportunity to develop a library that provides this function myself, and started the `Testcontainers Meilisearch` project.

## Project introduction

`Testcontainers Meilisearch` is currently registered on [Testcontainers official site](https://testcontainers.com/modules/meilisearch/) and has also been uploaded to the [Maven Central](https://central.sonatype.com/artifact/io.vanslog/testcontainers-meilisearch) repository. You can easily run the Meilisearch container and run test code with a simple dependency setup.

### Dependency settings

To use `Testcontainers Meilisearch` in test code, you must add dependency as follows.

#### Gradle

```groovy
testImplementation 'io.vanslog:testcontainers-meilisearch:1.0.0'
```

#### Maven

```xml
<dependency>
 <groupId>io.vanslog</groupId>
 <artifactId>testcontainers-meilisearch</artifactId>
 <version>1.0.0</version>
 <scope>test</scope>
</dependency>
```

### How to use

For example, if you are writing test code that requires logic to access Meilisearch, you can use `Testcontainers Meilisearch` in the following way.

```java
@Testcontainers
class MeilisearchContainerTest {

 @Container
 private final MeilisearchContainer container = new MeilisearchContainer()
 .withMasterKey("masterKey");

 @Test
 void shouldConnectToMeilisearch() throws MeilisearchException {
 Config config = new Config(
 "http://" + container.getHost() + ":" + container.getMappedPort(7700),
 "masterKey"
 );
 Client client = new Client(config);
 assertThat(client.isHealthy()).isTrue();
}
```

The example code above simply creates a Meilisearch client and checks whether it can communicate with the test container.
One thing to note here is that the test container binds ports randomly, so you need to get the port through `getMappedPort()`.
MeilisearchContainer uses port `7700` to run containers, so you can get the port via `getMappedPort(7700)`.

> If `Master Key` is not specified, it is generated randomly. Therefore, you must configure it to connect to the client.

## Project Review

Until now, there was a lack of understanding of Java's package ecosystem, but through this project, we were able to supplement this. I signed up for `Sonatype OSSRH`, linked it to Maven build settings, packaged and signed GPG, and then directly experienced the process of deploying to `Maven Central`. It was a complicated process, but I was able to proceed step by step by referring to the [Sonatype OSSRH Guide](https://central.sonatype.org/pages/ossrh-guide.html). Through this experience, we hope to continue developing various Java-based libraries.

The project is still in its early stages, so many features are lacking. We plan to continue to improve in the future, and if anyone would like to contribute directly, we would appreciate it if you could leave an issue or PR in the corresponding [GitHub repository](https://github.com/junghoon-vans/testcontainers-meilisearch). 🤗
