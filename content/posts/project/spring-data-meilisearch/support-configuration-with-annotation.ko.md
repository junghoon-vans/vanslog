---
title: "Spring Data Meilisearch #2 - 어노테이션 기반 설정"
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
  스프링 라이브러리에서 어노테이션 기반 설정을 지원하는 방법
---

이번 글에서는 자바 기반의 설정을 지원하는 방법에 대해 알아보겠습니다.
스프링 3.1 버전 이후부터는 기본적으로 `@Configuration` 어노테이션을 사용하여 설정하는 것을 권장하고 있습니다. 그렇기 때문에 저희 라이브러리에서도 `@Configuration` 어노테이션을 사용하여 설정할 수 있도록 지원하고 있습니다.

## 일반적인 어노테이션 기반 설정

일반적으로 `@Configuration` 어노테이션을 사용하여 설정하는 방법은 다음과 같습니다. Meilisearch `Client` 객체를 직접 생성하고 빈으로 등록한다면 다음과 같이 작성할 수 있습니다.

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

하지만 이러한 방식은 사용자가 직접 빈을 등록해야 하는 불편함이 있습니다. 그렇기 떄문에 저희 라이브러리에서는 표준화된 방식으로 설정할 수 있도록 지원하고 있습니다.

## 표준화된 설정을 제공하는 방법

우선 빈을 직접 등록하는 방식을 사용하지 않고, `clientConfiguration`이라는 메서드를 정의하여 설정할 수 있도록 지원하고 있습니다.

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

`ClientConfiguration` 객체를 빌더 패턴으로 생성하여 반환하면 됩니다.
빌더 자체가 설정에 필요한 다양한 메서드를 가지고 있기 때문에, 사용자가 원하는 설정을 쉽게 지정할 수 있습니다.

## 마치며

Meilisearch 클라이언트 설정에 필요한 구체적인 클래스인 `Config`와 `Client`를 캡슐화하여 사용자가 직접 생성할 필요가 없도록 하였습니다. 

덕분에 사용자는 `ClientConfiguration` 인터페이스만을 활용하여 설정할 수 있게 되었고 더욱 편리하게 사용할 수 있게 되었습니다.