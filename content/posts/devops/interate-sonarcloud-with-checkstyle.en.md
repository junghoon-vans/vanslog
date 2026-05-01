---
title: "Interate Sonarcloud With Checkstyle"
translationKey: posts/devops/interate-sonarcloud-with-checkstyle
date: 2023-01-15T13:56:13+09:00
draft: false
series:
  - General
categories:
  - DevOps
tags:
  - DND
  - SonarCloud
  - Checkstyle
summary: >
  How to check your coding style while using SonarCloud
---

Recently, I am in charge of the backend development of [Side Project](https://github.com/dnd-side-project/dnd-8th-8-backend) through an external activity called DND. Before starting the full-scale project, we are working on building a CI/CD pipeline.

We worked on linking `SonarCloud` and `Checkstyle` to statically analyze Spring-based backend code. In this article, we will discuss why this decision was made and how the integration works.

## SonarCloud

[SonarQube](https://www.sonarqube.org) is a `static analysis tool` used to discover bugs, code smells, and security vulnerabilities. When code is pushed to a repository or a PR is created, SonarQube can analyze the code and display the results. However, using SonarQube directly has the disadvantage of requiring you to run and maintain a server.

I thought it would be difficult to run the server myself, so I decided to use [SonarCloud](https://sonarcloud.io) instead. SonarCloud is a service that provides SonarQube as a cloud service. It supports `integration with code repositories` like GitHub and can be used for public repositories for free.

### SonarCloud Disadvantages

SonarCloud offers limited features compared to SonarQube. Typically, the `Linter` function, which checks `coding convention`, which is a rule that must be followed when writing code, is not provided by default.
Introducing a linter is essential to improve code readability and maintain code consistency. Therefore, we decided to use [Checkstyle](https://checkstyle.sourceforge.io) while using SonarCloud.

### Linkage with external analyzer reports

SonarCloud provides functionality that works with [External Analyzer Reports](https://docs.sonarcloud.io/enriching/external-analyzer-reports/). This function passes the results of `External Analyzer` to SonarCloud so that they can be analyzed together. This feature allows you to use Checkstyle in conjunction with SonarCloud.

## Integrating SonarCloud and Checkstyle

In this section, we will introduce in detail how to link `SonarCloud` and `Checkstyle`. This explanation assumes that `GitHub Actions` is used in conjunction with a `Gradle`-based project. If you want to apply it to a project using a different technology stack, please refer to this article and [SonarCloud official documentation](https://docs.sonarcloud.io/).

### Disable SonarCloud Auto Analysis

SonarCloud has a feature called `Auto Analysis` enabled by default. This feature automatically analyzes the code quality of the repository. This feature has the advantage of being very convenient to use, but it must be disabled in order to use external static analysis tools together.

To disable it, select the `General` tab in the `Settings` of the SonarCloud project linked to the storage and then disable [Auto Analysis](https://docs.sonarcloud.io/advanced-setup/automatic-analysis/#deactivating-automatic-analysis).

> If you are not an admin of SonarCloud, you cannot disable this feature. In this case, please contact the administrator to request deactivation.

### CI-based analysis setup

Now we need to set [CI-based analysis](https://docs.sonarcloud.io/advanced-setup/ci-based-analysis/overview/). CI-based analysis refers to the method of delivering analysis results to SonarCloud using CI tools such as `GitHub Actions` or `Travis CI`.

#### SonarQube plugin settings

Because SonarCloud uses the SonarQube plugin to perform analysis, you need to set up the SonarQube plugin.

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

The example above shows the settings in the `build.gradle` file of our project. `sonar.organization` and `sonar.projectKey` can be written by creating a project in SonarCloud and checking `Settings` of the project. `sonar.host.url` is the host URL that runs SonarQube. If you use SonarCloud, you can write `https://sonarcloud.io`.

> The SonarQube plugin is compatible with both SonarQube and SonarCloud. If you use SonarQube, you only need to change the host URL to SonarQube's host URL.
> In past versions, the task was named `sonarqube`, but it is now named `sonar`.

#### GitHub Actions settings

In this section, we will look at an example of delivering analysis results to SonarCloud using [GitHub Actions](https://docs.sonarcloud.io/advanced-setup/ci-based-analysis/github-actions-for-sonarcloud/). The example below shows the settings in the `.github/workflows/ci.yml` file of our project.

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

The example above requests analysis from SonarCloud when a PR is created or pushed to branch `main`. `GITHUB_TOKEN` and `SONAR_TOKEN` are the tokens to use in GitHub Actions. `GITHUB_TOKEN` does not need to be created separately, but `SONAR_TOKEN` must be created in `Settings` of the project in SonarCloud and registered in `GitHub Secrets`.

> In the case of a Gradle project, if you use [sonarcloud-github-action](https://github.com/sonarsource/sonarcloud-github-action), an error will occur asking you to use the Gradle plugin, so you must set it as in the example above.

### Checkstyle plugin settings

This section introduces how to set up the Checkstyle plugin. If you do not want to use the Checkstyle plugin, you can skip this section.

```groovy
plugins {
    id 'checkstyle'
}

checkstyle {
    toolVersion = "10.4"
}
```

The Checkstyle plugin is configured in the `build.gradle` file. The example above shows the settings in the `build.gradle` file of our project. `toolVersion` refers to the version of the Checkstyle plugin.

#### Checkstyle configuration file

To use the Checkstyle plugin, you must place the `checkstyle.xml` file in the `config/checkstyle` directory. Usually, [Checkstyle configuration file](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml) provided by Google is used to match Google's coding style. Depending on the nature of the project, you can use Sun Microsystems' [Checkstyle configuration file](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/sun_checks.xml) or customize it yourself.

### Checkstyle report path integration

Lastly, here is an example of linking SonarCloud and Checkstyle. If you have completed all of the previous settings, linking is very simple. Just add an attribute called `sonar.java.checkstyle.reportPaths`.

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

`sonar.java.checkstyle.reportPaths` passes the `main.xml` file generated by the Checkstyle plugin to SonarCloud. This setup allows SonarCloud to check warnings reported by the Checkstyle plugin.

## Conclusion

By linking SonarCloud and Checkstyle this way, you can manage code quality through the CI pipeline. However, it can also be integrated with other static analysis tools. For example, if you use `JaCoCo`, just add the `sonar.jacoco.reportPaths` property. If you want to manage code coverage by linking JaCoCo together, please refer to [Java test coverage](https://docs.sonarqube.org/latest/analyzing-source-code/test-coverage/java-test-coverage/).

Our project manages code quality by linking `SonarCloud`, `Checkstyle`, and `JaCoCo`. If you want to see the configuration file directly, please refer to our project's [GitHub Actions](https://github.com/dnd-side-project/dnd-8th-8-backend/blob/d8525a13afcb3160b1dcd244b65ccbc975a4c943/.github/workflows/ci.yml) configuration file or [build.gradle](https://github.com/dnd-side-project/dnd-8th-8-backend/blob/d8525a13afcb3160b1dcd244b65ccbc975a4c943/build.gradle) file.

## References

* [SonarCloud official site](https://sonarcloud.io/)
* [SonarCloud official documentation](https://docs.sonarcloud.io/)
* [SonarCloud GitHub Action](https://github.com/sonarsource/sonarcloud-github-action)
* [SonarCloud GitHub Action Sample - Gradle example](https://github.com/sonarsource/sonarcloud-github-action-samples/tree/gradle)
* [SonarCloud Gradle plugin official documentation](https://docs.sonarqube.org/latest/analyzing-source-code/scanners/sonarscanner-for-gradle/)
* [Checkstyle Gradle plugin official documentation](https://docs.gradle.org/current/userguide/checkstyle_plugin.html)
