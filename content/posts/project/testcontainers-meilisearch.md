---
title: "Meilisearch를 위한 Testcontainers 라이브러리 개발기"
date: 2023-07-21T22:47:52+09:00
draft: false
categories: 프로젝트
tags:
  - ☕️ Java
  - Testcontainers
  - Meilisearch
  - 라이브러리
summary: >
  Testcontainers Meilisearch를 소개하고, 개발하면서 느낀 점을 공유합니다.
---

프로젝트 동기
===

최근 진행중인 [Spring Data Meilisearch](/posts/project/spring-data-meilisearch/introduction) 프로젝트에서 [Meilisearch](https://www.meilisearch.com) 인스턴스에 접근하는 테스트 코드 작성이 필요했습니다. 다양한 방법을 고려하였지만 결국 `Testcontainers`를 사용하는 것이 가장 적합하다고 판단하였습니다. 하지만 Testcontainers에서 공식적으로는 Meilisearch를 지원하지 않았고, `GenericContainer`라는 클래스를 오버라이딩해야 했습니다. 그래서 이번 기회에 해당 기능을 제공하는 라이브러리를 직접 개발해보기로 결심하였고, `Testcontainers Meilisearch` 프로젝트를 시작하게 되었습니다.

프로젝트 소개
===

`Testcontainers Meilisearch`는 현재 [Testcontainers 공식 사이트](https://testcontainers.com/modules/meilisearch/)에 등록되어 있으며, [Maven Central](https://central.sonatype.com/artifact/io.vanslog/testcontainers-meilisearch) 레포지토리에도 업로드되어 있습니다. 간단한 의존성 설정만으로 손쉽게 Meilisearch 컨테이너를 동작시키고 테스트 코드를 수행할 수 있습니다.

의존성 설정
---

테스트 코드 상에서 `Testcontainers Meilisearch`를 사용하기 위해서는 다음과 같이 의존성을 추가해야 합니다.

### Gradle

```groovy
testImplementation 'io.vanslog:testcontainers-meilisearch:1.0.0'
```

### Maven

```xml
<dependency>
  <groupId>io.vanslog</groupId>
  <artifactId>testcontainers-meilisearch</artifactId>
  <version>1.0.0</version>
  <scope>test</scope>
</dependency>
```

사용법
---

예를 들어 Meilisearch에 접근하는 로직이 필요한 테스트 코드를 작성한다면, 다음과 같은 방법으로 `Testcontainers Meilisearch`를 사용할 수 있습니다.

```java
@Testcontainers
class MeilisearchContainerTest {

  @Container
  private final MeilisearchContainer container = new MeilisearchContainer()
    withMasterKey("masterKey");

  @Test
  void test() throws MeilisearchException {
    // write your test code
}
```

> `Master Key`를 지정하지 않을 경우 랜덤으로 생성됩니다. 테스트 코드 진행 시 클라이언트의 `Master Key`와 동일하게 설정해두시는 것이 좋습니다.

프로젝트 후기
===

이전까지는 자바의 패키지 생태계에 대한 이해가 부족하였지만, 이번 프로젝트를 통해 이러한 부분을 보완할 수 있었습니다. `Sonatype OSSRH`에 가입하고 Maven 빌드 설정에 연동하였고, 패키징과 GPG 서명을 한 이후 `Maven Central`에 배포하는 과정을 직접 경험해보았습니다. 복잡한 과정이었지만 [Sonatype OSSRH Guide](https://central.sonatype.org/pages/ossrh-guide.html)를 참고하면서 하나씩 진행해나갈 수 있었습니다. 이번 경험을 통해 앞으로도 다양한 자바 기반의 라이브러리 개발을 진행할 수 있으리라 기대합니다.

해당 프로젝트는 아직 초기 단계이므로 기능이 많이 부족한 상태입니다. 앞으로 지속적으로 개선해나갈 생각이며, 혹시라도 직접 기여를 해주실 분이 계시다면 해당 [Github 저장소](https://github.com/junghoon-vans/testcontainers-meilisearch)에 이슈나 PR을 남겨주시면 감사하겠습니다. 🤗
