---
title: "Support Configuration With Annotation"
translationKey: posts/project/spring-data-meilisearch/support-configuration-with-annotation
date: 2023-08-11T00:06:13+09:00
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
  How to support annotation based configuration in spring library
---


In this article, we will learn about how to support Java-based settings.
From Spring 3.1 version onwards, it is recommended to configure using the `@Configuration` annotation by default. That is why our library also supports configuration using the `@Configuration` annotation.

## General annotation-based configuration

In general, here's how to configure it using the `@Configuration` annotation: If you directly create a Meilisearch `Client` object and register it as a bean, you can write it as follows.

```java
@Configuration
public class MeilisearchConfiguration {
 @Bean
 public Client meilisearchClient() {
 Config meilisearchConfig = new Config(
 "http://localhost:7700",
 "masterKey"
 );
 return new Client(meilisearchConfig);
 }
}
```

However, this method has the inconvenience of requiring the user to register the beans themselves. That's why our library supports setting it up in a standardized way.

## How to provide standardized settings

First of all, rather than using the method of directly registering a bean, it supports configuration by defining a method called `clientConfiguration`.

```java
@Configuration
class CustomConfiguration extends MeilisearchConfiguration {

 @Override
 public ClientConfiguration clientConfiguration() {
 return ClientConfiguration.builder()
 .connectedToLocalhost()
 .withApiKey("masterKey")
 .build();
 }
}
```

Simply create and return a `ClientConfiguration` object using the builder pattern.
Because the builder itself has various methods for settings, users can easily specify the settings they want.

## Conclusion

We have encapsulated `Config` and `Client`, the specific classes required for Meilisearch client configuration, so that users do not need to create them themselves.

Thanks to this, users can configure settings using only the `ClientConfiguration` interface, making it more convenient to use.
