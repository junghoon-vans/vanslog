---
title: "공식 Actions를 활용한 GitHub Pages 배포"
date: 2023-01-21T21:50:42+09:00
draft: false
series: []
categories: 
  - devops
tags:
  - GitHub
  - GitHub Actions
  - GitHub Pages
  - Spring REST Docs
summary: >
  공식 Actions를 활용한 GitHub Pages 배포 방법을 소개합니다.
---

[DND](https://www.dnd.ac)에서 진행하는 프로젝트에서 [Spring Rest Docs](https://spring.io/projects/spring-restdocs)를 사용하여 `API 문서화`를 진행하고 있으며, 이렇게 만들어진 문서화 페이지는 [GitHub Pages](https://pages.github.com/)를 통해 배포하고 있습니다. GitHub Pages는 워낙 잘 알려진 서비스이므로 이를 통해 배포하는 방법은 다양한 자료들이 존재합니다. 하지만 공식 액션을 사용하는 방법은 아직까지 잘 알려지지 않은 것 같아 이를 소개하고자 합니다.

# GitHub Pages

GitHub Pages는 GitHub에서 제공하는 `정적 웹사이트 호스팅 서비스`입니다. 무료로 제공되는 서비스이므로 간단한 웹사이트를 호스팅하기에 적합합니다. 사이드 프로젝트를 진행하는 과정에서 프론트 팀과 API 문서를 공유하기에 적합합니다.

## Branch를 통한 배포

일반적으로 `gh-pages`라는 이름의 브랜치를 통해 배포하는 방식을 사용합니다. 페이지의 정적 파일을 해당 브랜치에 업로드하는 것으로 배포를 진행합니다. 이번 포스팅에서는 `GitHub Actions`를 사용하여 배포하는 방법을 중점적으로 소개할 예정이므로 브랜치를 이용한 배포에 대한 자세한 정보는 [GitHub Pages 공식 문서](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-from-a-branch)를 참고하시기 바랍니다. 

# GitHub Actions를 사용한 배포

![GitHub Pages Deployment](https://user-images.githubusercontent.com/44942700/213869054-42ebba23-e368-4906-8f9b-d82e45fdca9b.png)

최근 GitHub는 `GitHub Pages`를 배포하기 위해 Actions을 사용하는 방법을 추가하였습니다. 액션을 사용하여 페이지를 배포하는 것의 장점은 배포하는 작업을 쉽게 자동화할 수 있으며, 별도의 브랜치를 생성하지 않아도 된다는 점입니다. 이전까지 비공식적으로 지원하던 액션인 [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)를 사용중이셨다면 공식 액션으로 마이그레이션하는 것을 고려해보시기 바랍니다. 

> 💡 해당 기능은 베타 버전이므로 프로젝트 성격에 따라 사용 여부를 결정하시기 바랍니다.

## 구성 요소

GitHub Actions를 사용한 GitHub Pages 배포 방법은 다음과 같은 액션들이 사용됩니다.

- [actions/configure-pages](https://github.com/actions/configure-pages)
  - GitHub Pages를 배포하기 위한 환경을 설정
- [actions/upload-pages-artifact](https://github.com/actions/upload-pages-artifact)
  - GitHub Pages를 배포하기 위한 빌드 결과물을 업로드
- [actions/deploy-pages](https://github.com/actions/deploy-pages)
  - GitHub Pages 배포

## 예제 코드

아래 예제는 Spring REST Docs를 사용하여 API 문서를 생성하고 GitHub Pages에 배포하는 예제입니다. [GitHub Starter Workflows](https://github.com/actions/starter-workflows/blob/main/pages/static.yml)에서 제공하는 코드를 기반으로 작성되었습니다.

```yaml
name: API Docs
on:
  push:
    branches:
      - main
  pull_request:

permissions: # 권한 설정
  contents: read # for actions/configure-pages
  pages: write # for actions/deploy-pages
  id-token: write # for actions/deploy-pages

concurrency: # 동시 실행 제한
  group: "pages"
  cancel-in-progress: true

jobs:
  deploy:
    name: API Docs
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }} # 배포된 페이지의 URL
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Set up Pages
        uses: actions/configure-pages@v2 
      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          java-version: 17
          distribution: 'zulu'
          cache: 'gradle'
      - name: Build Asciidoc
        run: ./gradlew asciidoctor
      - name: Upload pages artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: './build/docs/asciidoc'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```

## 에러 해결

> **Branch "<브랜치명>" is not allowed to deploy to github-pages due to environment protection rules.**

GitHub Pages를 배포하는 과정에서 다음과 같은 에러가 발생할 수 있습니다. 이는 특정 브랜치에 대해서 배포가 제한되어 있는 경우 발생합니다.
이를 해결하는 방법은 프로젝트 내 `Settings > Environments > github-pages > Deployment branches` 에서 `허용할 브랜치`를 지정하거나 `All branches`를 선택해주시면 됩니다.


# 마치며

지금까지 공식 GitHub Actions를 사용하여 GitHub Pages에 배포하는 방법에 대해 알아보았습니다. 이러한 방법을 사용하여 스프링 부트의 API 문서를 생성하고 GitHub Pages에 배포하는 과정을 자동화할 수 있었습니다.

# 참고 문헌

- [GitHub Pages 공식 문서](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [actions/configure-pages](https://github.com/actions/configure-pages)
- [actions/upload-pages-artifact](https://github.com/actions/upload-pages-artifact)
- [actions/deploy-pages](https://github.com/actions/deploy-pages)
- [GitHub Starter Workflows](https://github.com/actions/starter-workflows/blob/main/pages/static.yml)
