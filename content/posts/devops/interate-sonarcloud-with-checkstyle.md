---
title: "SonarCloud와 Checkstyle을 통합하여 사용하기"
date: 2023-01-15T13:56:13+09:00
draft: false
series:
  - 자바 정적 분석 도구
categories:
  - DevOps
tags:
  - DND
  - SonarCloud
  - Checkstyle
summary: >
  SonarCloud를 사용하면서 코딩 스타일을 체크하는 방법
---

최근 DND라는 대외활동을 통해 [사이드 프로젝트](https://github.com/dnd-side-project/dnd-8th-8-backend)의 백엔드 개발을 맡고 있습니다. 본격적인 프로젝트 시작에 앞서 CI/CD 파이프라인을 구축하는 작업을 진행하고 있습니다.

스프링 기반의 백엔드 코드를 정적 분석하기 위해 `SonarCloud`와 `Checkstyle`을 연동하는 작업을 진행하였습니다. 이 글에서는 왜 이러한 결정을 내리게 되었는지, 그리고 연동은 어떻게 이루어지는지에 대해 다루도록 하겠습니다.

## SonarCloud

[SonarQube](https://www.sonarqube.org)는 버그, 코드 스멜, 보안 취약점을 발견하기 위해 사용하는 `정적 분석 도구`입니다. 저장소에 코드가 푸시되거나 PR이 생성되면 SonarQube가 코드를 분석하여 결과를 보여주는 기능을 제공합니다. 다만 SonarQube를 사용하기 위해서는 이것을 구동하는 `서버를 운영`해야 한다는 단점이 있습니다.

직접 서버를 운영하기에는 무리가 있다는 생각에 [SonarCloud](https://sonarcloud.io)를 대신 사용하기로 결정하였습니다. SonarCloud는 SonarQube를 클라우드 서비스로 제공하는 서비스로, GitHub과 같은 `코드 저장소와 연동`을 지원하며 Public 저장소에 대해서는 무료로 사용할 수 있기 때문입니다.

### SonarCloud 단점

SonarCloud는 SonarQube에 비해 제한된 기능을 제공합니다. 대표적으로 코드를 작성할 때 지켜야 하는 규칙인 `코딩 컨벤션`을 체크해주는 `Linter` 기능이 기본적으로 제공되지 않습니다.
코드의 가독성을 높이고, 코드의 일관성을 유지하기 위해 Linter를 도입하는 것은 필수적입니다. 그러므로 SonarCloud를 사용하면서도 [Checkstyle](https://checkstyle.sourceforge.io)를 함께 사용하기로 결정하게 되었습니다.

### 외부 분석기 리포트 연동

SonarCloud는 [External Analyzer Reports](https://docs.sonarcloud.io/enriching/external-analyzer-reports/)와 연동하는 기능을 제공합니다. 해당 기능은 `외부 분석기`의 결과를 SonarCloud에 전달하여 함께 분석할 수 있도록 해줍니다. 해당 기능을 이용하면 Checkstyle을 SonarCloud와 연동하여 사용할 수 있습니다.

## SonarCloud와 Checkstyle 연동하기

이번 목차에서는 `SonarCloud`와 `Checkstyle`의 연동을 진행한 방법에 대해 자세히 소개해드리도록 하겠습니다. 해당 설명에서는 `Gradle` 기반 프로젝트에 `GitHub Actions`를 함께 사용하는 경우를 가정하고 있습니다. 만약 다른 기술 스택을 사용한 프로젝트에 적용하고 싶으시다면 해당 글과 [SonarCloud 공식 문서](https://docs.sonarcloud.io/)를 함께 참고하시기 바랍니다.

### SonarCloud Auto Analysis 비활성화

SonarCloud는 기본적으로 `Auto Analysis`라는 기능이 활성화되어 있습니다. 이 기능은 저장소의 코드 품질을 자동으로 분석해주는 기능입니다. 이 기능은 매우 편리하게 사용할 수 있다는 장점이 있으나, 외부 정적 분석 도구를 함께 사용하기 위해서는 이 기능을 비활성화해야 합니다.

비활성화하는 방법은 저장소와 연동된 SonarCloud 프로젝트의 `Settings`에서 `General` 탭을 선택한 후 [Auto Analysis](https://docs.sonarcloud.io/advanced-setup/automatic-analysis/#deactivating-automatic-analysis)를 비활성화하는 것입니다.

> 만약 본인이 SonarCloud의 어드민이 아니라면 해당 기능을 비활성화할 수 없습니다. 이 경우 어드민에게 요청하여 비활성화를 요청하시기 바랍니다.

### CI 기반 분석 설정

이제  [CI 기반 분석](https://docs.sonarcloud.io/advanced-setup/ci-based-analysis/overview/)을 설정해야 합니다. CI 기반 분석이란 `GitHub Actions`이나 `Travis CI`와 같은 CI 툴을 사용하여 SonarCloud에 분석 결과를 전달하는 방법을 말합니다.

#### SonarQube 플러그인 설정

SonarCloud는 SonarQube 플러그인을 사용하여 분석을 수행하기 때문에 SonarQube 플러그인을 설정해야 합니다.

```groovy
plugins {
    id 'org.sonarqube' version '3.5.0.2730'
}

sonar {
    properties {
        property 'sonar.host.url', 'https://sonarcloud.io'
        property 'sonar.organization', 'dnd-side-project'
        property 'sonar.projectKey', 'dnd-side-project_dnd-8th-8-backend'
    }
}
```

위 예제는 저희 프로젝트의 `build.gradle` 파일에 설정한 내용입니다. `sonar.organization`과 `sonar.projectKey`는 SonarCloud에서 프로젝트를 생성한 후 프로젝트의 `Settings`에서 확인하여 작성해주시면 됩니다. `sonar.host.url`은 SonarQube를 동작시키는 호스트 URL로 SonarCloud를 사용한다면 `https://sonarcloud.io`로 작성하시면 됩니다. 

> SonarQube 플러그인은 SonarQube와 SonarCloud 모두 호환됩니다. 만약 SonarQube를 사용한다면 호스트 URL만 SonarQube의 호스트 URL로 수정하면 됩니다.

> 과거 버전에서는 sonarqube라는 이름으로 task를 설정하였지만 현재는 `sonar`라는 이름으로 task를 설정해야 합니다.

#### GitHub Actions 설정

이번 목차에서는 [GitHub Actions](https://docs.sonarcloud.io/advanced-setup/ci-based-analysis/github-actions-for-sonarcloud/)를 이용하여 SonarCloud에 분석 결과를 전달하는 예제를 살펴보겠습니다. 아래 예제는 저희 프로젝트의 `.github/workflows/ci.yml` 파일에 설정한 내용입니다.

```yaml
name: CI
on:
  push:
    branches:
      - main
  pull_request:
jobs:
  sonarcloud:
    name: SonarCloud
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          java-version: 17
          distribution: 'zulu'
      - name: Cache Gradle packages
        uses: actions/cache@v3
        with:
          path: ~/.gradle/caches
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle') }}
          restore-keys: ${{ runner.os }}-gradle
      - name: Cache SonarCloud packages
        uses: actions/cache@v3
        with:
          path: ~/.sonar/cache
          key: ${{ runner.os }}-sonar
          restore-keys: ${{ runner.os }}-sonar
      - name: Build and analyze
        run: ./gradlew build jacocoTestReport sonar --info
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

위 예제는 `main` 브랜치에 푸시되거나 PR이 생성될 때 SonarCloud에 분석을 요청합니다. `GITHUB_TOKEN`과 `SONAR_TOKEN`은 GitHub Actions에서 사용할 토큰입니다. `GITHUB_TOKEN`은 별도로 생성할 필요가 없지만, `SONAR_TOKEN`은 SonarCloud에서 프로젝트의 `Settings`에서 생성하고 `GitHub Secrets`에 등록해야 합니다.

> Gradle 프로젝트인 경우 [sonarcloud-github-action](https://github.com/sonarsource/sonarcloud-github-action)을 사용하면 Gradle 플러그인을 사용하라는 에러가 발생하므로 반드시 위 예제와 같이 설정해야 합니다.

### Checkstyle 플러그인 설정

이번 목차에서는 Checkstyle 플러그인을 설정하는 방법을 소개합니다. 만약 Checkstyle 플러그인을 사용하지 않기를 원하신다면 이번 목차는 건너뛰어도 됩니다.

```groovy
plugins {
    id 'checkstyle'
}

checkstyle {
    toolVersion = "10.4"
}
```

Checkstyle 플러그인은 `build.gradle` 파일에 설정합니다. 위 예제는 저희 프로젝트의 `build.gradle` 파일에 설정한 내용입니다. `toolVersion`은 Checkstyle 플러그인의 버전을 의미합니다.

#### Checstyle 설정 파일

Checkstyle 플러그인을 사용하기 위해서는 `checkstyle.xml` 파일을 `config/checkstyle` 디렉토리에 위치시켜야 합니다. 보통 구글의 코딩 스타일에 맞추기 위해 구글이 제공하는 [Checkstyle 설정 파일](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml)을 사용합니다. 프로젝트 성격에 따라서 썬 마이크로시스템즈의 [Checkstyle 설정 파일](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/sun_checks.xml)을 사용하거나 직접 커스텀하여 사용할 수도 있습니다.

### Checkstyle 리포트 경로 연동

마지막으로 SonarCloud와 Checkstyle을 연동하는 예제를 소개합니다. 앞선 설정을 모두 완료했다면 매우 간단하게 연동할 수 있습니다. `sonar.java.checkstyle.reportPaths`라는 속성만 추가하면 됩니다.

```groovy
plugins {
    id 'checkstyle'
    id 'org.sonarqube' version '3.5.0.2730'
}

checkstyle {
    maxWarnings = 0
    toolVersion = "10.4"
}

sonar {
    properties {
        property 'sonar.host.url', 'https://sonarcloud.io'
        property 'sonar.organization', 'dnd-side-project'
        property 'sonar.projectKey', 'dnd-side-project_dnd-8th-8-backend'
        property 'sonar.java.checkstyle.reportPaths', 'build/reports/checkstyle/main.xml'
    }
}
```

`sonar.java.checkstyle.reportPaths`는 Checkstyle 플러그인이 생성한 `main.xml` 파일을 SonarCloud에 전달합니다. 이렇게 설정하면 SonarCloud에서 Checkstyle 플러그인의 경고를 확인할 수 있습니다.

## 마치며

이렇게 SonarCloud와 Checkstyle을 연동하여 코드 품질을 관리할 수 있습니다. 이번 글에서는 SonarCloud와 Checkstyle을 연동하는 방법을 소개했습니다. 하지만 다른 정적 분석 도구와도 연동할 수 있습니다. 예를 들어 `JaCoCo`를 사용한다면 `sonar.jacoco.reportPaths` 속성을 추가하면 됩니다. 만약 JaCoCo를 함께 연동하여 코드 커버리지를 관리하고 싶다면 [Java test coverage](https://docs.sonarqube.org/latest/analyzing-source-code/test-coverage/java-test-coverage/)를 참고하시면 됩니다.

저희 프로젝트는 `SonarCloud`와 `Checkstyle`, `JaCoCo`를 연동하여 코드 품질을 관리하고 있습니다. 만약 설정 파일을 직접 보고 싶으시다면 저희 프로젝트의 [GitHub Actions](https://github.com/dnd-side-project/dnd-8th-8-backend/blob/d8525a13afcb3160b1dcd244b65ccbc975a4c943/.github/workflows/ci.yml) 설정 파일 혹은 [build.gradle](https://github.com/dnd-side-project/dnd-8th-8-backend/blob/d8525a13afcb3160b1dcd244b65ccbc975a4c943/build.gradle) 파일을 참고해주세요.

## 참고 문헌

  * [SonarCloud 공식 사이트](https://sonarcloud.io/)
  * [SonarCloud 공식 문서](https://docs.sonarcloud.io/)
  * [SonarCloud GitHub Action](https://github.com/sonarsource/sonarcloud-github-action)
  * [SonarCloud Github Action Sample - Gradle 예제](https://github.com/sonarsource/sonarcloud-github-action-samples/tree/gradle)
  * [SonarCloud Gradle 플러그인 공식 문서](https://docs.sonarqube.org/latest/analyzing-source-code/scanners/sonarscanner-for-gradle/)
  * [Checkstyle Gradle 플러그인 공식 문서](https://docs.gradle.org/current/userguide/checkstyle_plugin.html)
