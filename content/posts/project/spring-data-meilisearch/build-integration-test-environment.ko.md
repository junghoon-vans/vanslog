---
title: "Spring Data Meilisearch #3 - 통합테스트 환경 구축"
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
  Testcontainers와 JUnit 5 확장 모델로 통합 테스트 환경 구축하기
---

통합 테스트는 [Spring Data Meilisearch](https://github.com/junghoon-vans/spring-data-meilisearch)의 개발을 진행하는 과정에서 매우 중요한 부분입니다.
프로젝트의 핵심은 실제 Meilisearch와 통합되어 제대로 동작하는 지가 관건이기 때문입니다.
이번 포스트에서는 `MeilisearchTemplate`라는 실제 프로젝트 Core 모듈을 테스트하기 위해 적용한 방법에 대해서 소개해드리고자 합니다.

> Spring Data 구현체에서 `Template` 클래스는 데이터 저장소에 접근하기 위한 기본적인 메서드를 제공하는 클래스를 말합니다.

## Testcontainers Meilisearch

통합 테스트에서 가장 먼저 필요한 작업은 `Spring Data Meilisearch`가 접근할 인스턴스를 실행하는 것이었습니다.
인스턴스를 실행하는 방법에는 여러 가지가 있겠지만, 개발 환경에 구애받지 않는 테스트 환경을 구축하기 위해서 `Testcontainers`를 사용하였습니다.

### 컨테이너 실행

[Testcontainers Meilisearch](https://testcontainers.com/modules/meilisearch/) 라이브러리를 사용하면 Meilisearch에 필요한 테스트 환경을 쉽게 구축할 수 있습니다. `Testcontainers Meilisearch`는 Testcontainers 커뮤니티 모듈로 제공되는 라이브러리로, 현재 제가 직접 개발 및 유지보수하고 있는 프로젝트입니다.

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

위 예시 코드는 Meilisearch의 마스터 키를 설정하고 컨테이너를 실행합니다.

### 컨테이너 연결 설정

이제 테스트 코드 상에서 Meilisearch 인스턴스와 연결해봅시다.
`Spring Data Meilisearch`는 Meilisearch 인스턴스와 연결하는 방법으로 [JavaConfig](/posts/project/spring-data-meilisearch/support-configuration-with-annotation)와 [XML Namespace](/posts/project/spring-data-meilisearch/support-configuration-with-namespace)의 설정 방식을 모두 지원하지만, 이번 테스트 환경을 구축하는데 필요한 설정은 JavaConfig 방식으로 작성하였습니다.

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

위 코드에서는 `connectedTo` 메서드를 사용하여 Meilisearch 인스턴스의 호스트명과 포트를 지정합니다.
Testcontainers를 통해 실행한 컨테이너의 외부 포트는 매번 랜덤한 값으로 바인딩됩니다.
따라서 `meilisearch.getMappedPort(7700)`와 같이 컨테이너의 포트를 런타임 시 동적으로 가져와야 합니다.

### 전체 예시 코드

이제 전체적인 코드를 살펴보겠습니다. `MeilisearchTemplate` 클래스를 테스트하는 코드의 구조는 다음과 같습니다.

> 컨테이너 설정과 관련된 부분을 제외하고는 생략하였습니다.

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = { MeilisearchTemplateTest.Config.class })
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

`@ContextConfiguration` 어노테이션을 사용하여 이너클래스인 `Config`를 설정으로 지정합니다.
이를 통해 Testcontainers Meilisearch를 통해 실행한 컨테이너에 연결하고 테스트를 진행할 수 있습니다.

하지만 이와 같은 방법에도 여전히 문제가 있습니다.
테스트 파일이 많아진다면 `@BeforeAll` 어노테이션을 사용하여 컨테이너를 실행하는 코드를 중복해서 작성해야 하며, 중복으로 설정 파일을 생성해야 합니다.

## 관점 지향 프로그래밍

여기서 생각해볼만한 주제는 `관점 지향 프로그래밍(AOP)`입니다.

관점 지향 프로그래밍은 `관심사의 분리`를 통해 모듈성을 증가시키는 프로그래밍 기법입니다.
여기서 관심사는 `핵심`적인 기능과 `부가`적인 기능으로 나누어집니다.

![관점 지향 프로그래밍](https://cdn.codegym.cc/images/article/905659f3-ca60-47ed-847d-e3184c26b152/512.jpeg)

예를 들어 서로 다른 모듈들이 공통적으로 사용하는 기능이 있다면, 이는 부가적인 기능에 해당합니다.
로깅이나 보안과 같은 기능은 여러 모듈에 전역적으로 사용되기 때문에 `횡단 관심사`라고도 합니다.

### 테스트 코드의 관심사 분리

이전 챕터에서 살펴봤던 [MeilisearchTemplateTest](#전체-예시-코드) 코드를 다시 한 번 떠올려봅시다.
해당 코드는 MeilisearchTemplate의 기능이 제대로 동작하는 지 확인하기 위한 통합 테스트 코드였습니다.
해당 코드를 AOP 관점에서 바라보면 다음과 같이 나눌 수 있을 것입니다.

- `핵심 관심사`: 테스트 수행
- `횡단 관심사`: 컨테이너 실행 및 관리

만약 횡단 관심사인 컨테이너 관리를 모듈로 분리할 수 있다면 어떨까요?
테스트 코드 상에서는 테스트 수행에만 집중하게 되어 더욱 모듈성이 높은 코드를 작성할 수 있을 것입니다.
또한 여러 테스트 파일에서 컨테이너 관리에 대한 로직을 중복해서 작성할 필요도 없어질 것입니다.

## JUnit 5 확장 모델

JUnit 5를 이용하여 테스트 코드를 작성하는 경우 [확장 모델(Extension Model)](https://junit.org/junit5/docs/snapshot/user-guide/#extensions)을 통해 횡단 관심사를 모듈로 분리해낼 수 있습니다. 확장 모델을 이용하면 기존 JUnit 5에서 제공하는 기능을 확장하여 사용할 수 있습니다.

가장 대표적인 확장 모델은 Spring Framework에서 제공하는 `SpringExtension` 클래스입니다.
해당 클래스는 Spring Framework의 테스트 컨텍스트를 생성하고 관리하는 역할을 하며, 앞서 살펴본 예시 코드에서도 은연중에 사용되었습니다.

이것과 유사한 방식으로 재사용 가능한 확장 모델을 직접 만들어 사용할 수 있습니다.

### 확장 모델 구현

앞서 살펴본 예시 코드에서는 `@BeforeAll` 어노테이션을 사용하여 컨테이너를 실행하였습니다.
이를 확장 모델을 통해 구현하면 다음과 같습니다. 

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

`MeilisearchExtension` 클래스는 `BeforeAllCallback` 인터페이스를 구현하며, 이를 통해 `@BeforeAll` 어노테이션에 정의하던 기능을 여기서 구현할 수 있습니다.

> `MeilisearchConnection` 클래스는 컨테이너를 실행하고 연결하는 역할을 합니다. 전체 코드가 궁금하시다면 [레포지토리](https://github.com/junghoon-vans/spring-data-meilisearch/tree/main/src/test/java/io/vanslog/spring/data/meilisearch/junit/jupiter)를 참고해주세요.

### TestConfiguration 구현

이제 실행된 컨테이너의 호스트명과 포트를 가져온 설정을 제공하는 `MeilisearchTestConfiguration` 클래스를 구현해보겠습니다.

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

`MeilisearchTestConfiguration` 클래스는 앞서 `MeilisearchExtension` 클래스에서 실행한 컨테이너의 호스트명과 포트를 가져와 Meilisearch 인스턴스에 연결할 수 있는 설정을 제공합니다.

```java
@ExtendWith(SpringExtension.class)
@ExtendWith(MeilisearchExtension.class)
@ContextConfiguration(classes = { MeilisearchTestConfiguration.class })
class MeilisearchTemplateTest {
  // ...
}
```

이제 더 이상 Meilisearch 컨테이너를 생성하고 연결하는 로직을 매 테스트마다 작성할 필요가 없어졌습니다.
`MeilisearchExtension` 클래스를 통해 컨테이너를 실행하고, `MeilisearchTestConfiguration` 클래스를 통해 컨테이너에 연결할 수 있는 설정을 제공받아 테스트를 진행할 수 있습니다.

### 테스트 어노테이션 구현

여기까지만 진행해도 충분하지만 추가적으로 `@MeilisearchTest` 어노테이션을 생성하여 테스트 코드를 더욱 간결하게 작성할 수 있도록 해보겠습니다.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@ExtendWith(MeilisearchExtension.class)
@ExtendWith(SpringExtension.class)
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
public @interface MeilisearchTest {
}
```

`@MeilisearchTest` 어노테이션은 `MeilisearchExtension` 클래스를 통해 컨테이너를 실행하고, `SpringExtension` 클래스를 통해 스프링 컨텍스트를 생성합니다.

```java
@MeilisearchTest
@ContextConfiguration(classes = { MeilisearchTestConfiguration.class })
class MeilisearchTemplateTest {
  // ...
}
```

이제 더 이상 `@ExtendWith` 어노테이션을 사용할 필요가 없어졌으며, 간단하게 `@MeilisearchTest` 어노테이션만을 사용하여 테스트를 진행할 수 있습니다.

## 마치며

이번 포스트에서는 통합 테스트 환경을 구축하기 위해 사용한 방법들에 대해서 소개해드렸습니다.
직접 구현한 `Testcontainers` 라이브러리와 JUnit 5의 `확장 모델`로 구축한 환경인 만큼 뜻깊고 의미 있는 경험이었습니다. 

테스트 코드를 어떻게 작성하는 것이 좋은 방법인지에 대해서 많은 고민을 하였고, 이러한 시간 속에서 `관점 지향 프로그래밍`과도 연관지어 생각해볼 수 있었습니다.

Meilisearch 생태계의 부족한 스프링 통합 환경을 만들어 나가자는 [프로젝트 초기 목표](/posts/project/spring-data-meilisearch/introduction)가 점차 달성되어 가는 것이 뿌듯합니다.
앞으로도 더욱 많은 기능을 추가하여 사용자가 편리하게 사용할 수 있도록 노력하겠습니다.

## 참고문헌

- [Testcontainers Modules - Meilisearch](https://testcontainers.com/modules/meilisearch/)
- [What is AOP? Principles of aspect-oriented programming](https://codegym.cc/groups/posts/543-what-is-aop-principles-of-aspect-oriented-programming)
- [JUnit 5 공식 문서 - 확장 모델](https://junit.org/junit5/docs/snapshot/user-guide/#extensions)
