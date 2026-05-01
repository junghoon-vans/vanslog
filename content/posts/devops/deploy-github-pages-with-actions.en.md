---
title: "Deploy GitHub Pages With Actions"
translationKey: posts/devops/deploy-github-pages-with-actions
date: 2023-01-21T21:50:42+09:00
draft: false
series:
categories:
  - Devops
tags:
  - GitHub
  - GitHub Actions
  - GitHub Pages
  - Spring REST Docs
summary: >
  Introducing how to deploy GitHub Pages using official Actions.
---

In the project conducted by [DND](https://www.dnd.ac), `API documentation` is being carried out using [Spring Rest Docs](https://spring.io/projects/spring-restdocs), and the documentation pages created in this way are distributed through [GitHub Pages](https://pages.github.com/). Since GitHub Pages is such a well-known service, there are various materials available on how to distribute it through it. However, it seems that the method of using official actions is not yet well known, so I would like to introduce it.

## GitHub Pages

GitHub Pages is `static website hosting service` powered by GitHub. Since it is a free service, it is suitable for hosting simple websites. It is suitable for sharing API documentation with the front team while working on a side project.

### Distribution through Branch

Typically, deployment is done through a branch named `gh-pages`. Deployment is done by uploading the page's static files to the corresponding branch. In this post, we will focus on how to deploy using `GitHub Actions`, so please refer to [GitHub Pages official documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-from-a-branch) for more information about deploying using branches.

## Deployment using GitHub Actions

![GitHub Pages Deployment](https://user-images.githubusercontent.com/44942700/213869054-42ebba23-e368-4906-8f9b-d82e45fdca9b.png)

Recently GitHub added a way to use Actions to deploy `GitHub Pages`. The advantage of deploying pages using actions is that you can easily automate the deployment task and don't have to create a separate branch. If you were using the unofficially supported action [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages), please consider migrating to the official action.

> 💡 This feature is in beta, so please decide whether to use it or not depending on the nature of your project.

### Component

The following actions are used to deploy GitHub Pages using GitHub Actions.

- [actions/configure-pages](https://github.com/actions/configure-pages)
 - Set up the environment to deploy GitHub Pages
- [actions/upload-pages-artifact](https://github.com/actions/upload-pages-artifact)
 - Upload build results to distribute GitHub Pages
- [actions/deploy-pages](https://github.com/actions/deploy-pages)
 - Distribution of GitHub Pages

### Example codeThe example below uses Spring REST Docs to generate API documentation and deploy it to GitHub Pages. Written based on code provided by [GitHub Starter Workflows](https://github.com/actions/starter-workflows/blob/main/pages/static.yml).

```yaml
name: API Docs
on:
  push:
    branches:
      - main
  pull_request:

permissions: # permission settings
  contents: read # for actions/configure-pages
  pages: write # for actions/deploy-pages
  id-token: write # for actions/deploy-pages

concurrency: # limit concurrent runs
  group: "pages"
  cancel-in-progress: true

jobs:
  deploy:
    name: API Docs
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }} # URL of the deployed page
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

### Error resolution

> **Branch "<Branch name>" is not allowed to deploy to github-pages due to environment protection rules.**

The following errors may occur during deployment of GitHub Pages. This occurs when deployment is restricted to a specific branch.
To resolve this, specify `Branches to allow` in `Settings > Environments > github-pages > Deployment branches` or select `All branches` in the project.


## Conclusion

Now you've learned how to deploy to GitHub Pages using official GitHub Actions. Using these methods, we were able to automate the process of generating Spring Boot's API documentation and deploying it to GitHub Pages.

## References

- [GitHub Pages official documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [actions/configure-pages](https://github.com/actions/configure-pages)
- [actions/upload-pages-artifact](https://github.com/actions/upload-pages-artifact)
- [actions/deploy-pages](https://github.com/actions/deploy-pages)
- [GitHub Starter Workflows](https://github.com/actions/starter-workflows/blob/main/pages/static.yml)
