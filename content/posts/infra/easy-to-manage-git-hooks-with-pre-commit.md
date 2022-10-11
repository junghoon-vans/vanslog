---
title: "pre-commit로 Git Hooks 쉽게 관리하기"
date: 2022-10-12T00:15:32+09:00
draft: false
categories: Infra
tags:
  - pre-commit
  - git hooks
---

Git Hooks
---

`Git Hooks`는 깃에 이벤트가 발생했을 때 실행되는 스크립트입니다. `.git/hooks` 디렉터리에 스크립트를 작성해서 사용하는데, 이를 통해 커밋 직전에 코드컨벤션을 검사하거나 테스트코드를 실행해볼 수 있습니다.

### 문제점

혼자 개발하는 경우에는 이러한 방법이 나쁘지 않을 수도 있겠지만, 여럿이 개발에 참여하는 경우 아래와 같은 문제가 발생할 수 있습니다.

1. hook 스크립트 공유의 어려움.
2. 모두가 동일한 버전의 hook을 사용한다는 보장이 없음.

pre-commit 적용
---

[pre-commit](https://pre-commit.com)은 이러한 문제를 쉽게 해결해주는 **좋은 솔루션**입니다. 프로젝트 내에 설정 파일을 통해 hooks의 버전을 관리할 수 있으며, 이것들을 손쉽게 로컬머신에 설치할 수 있습니다.

### pre-commit 설치
   
```bash
$ pip install pre-commit  # pip로 설치
$ brew install pre-commit  # homebrew로 설치 
```

### 설정 파일 생성

`.pre-commit-config.yaml`이라는 파일을 프로젝트 루트 경로에 생성합니다. 이후 필요한 hooks 목록을 아래와 같이 작성합니다.


```yaml
repos:
  - repo: https://github.com/asottile/setup-cfg-fmt
    rev: v2.0.0
    hooks:
    - id: setup-cfg-fmt
  - repo: https://github.com/asottile/reorder_python_imports
    rev: v3.8.3
    hooks:
    - id: reorder-python-imports
  - repo: https://github.com/asottile/add-trailing-comma
    rev: v2.3.0
    hooks:
    - id: add-trailing-comma
```

> 지원되는 hooks 목록은 [해당 사이트](https://pre-commit.com/hooks.html)에서 검색해볼 수 있습니다.

### hook 스크립트 설치

아래 명령어는 위에서 정의한 hooks를 실제로 `.git/hooks` 경로의 `pre-commit` 파일에 적용해줍니다.

```bash
$ pre-commit install
```

pre-commit 실행
---

### 로컬머신

위 과정을 거쳐 설정을 모두 마쳤다면 커밋을 할 때마다 지정한 hooks가 순차적으로 동작할 것입니다. 만약 커밋 없이 강제로 실행하고 싶다면 다음 명령어를 사용하면 됩니다. 

```bash
$ pre-commit run --all-files
```


![pre-commit-example](/images/infra/pre-commit-example.png#center)

위 이미지는 제가 [checkstyle-cli](https://github.com/junghoon-vans/checkstyle-cli)라는 파이썬 패키지를 개발하는 과정에서 동작하는 화면입니다. 여러 `linter`나 `formatter`를 커밋을 할 때마다 동작시키는 과정을 통해 소스코드의 스타일을 일관되게 작성할 수 있습니다.

### pre-commit.ci

[pre-commit.ci](https://pre-commit.ci)는 깃헙에 소스코드가 push될 때 `pre-commit hook`을 실행해주는 서비스입니다. 깃헙 레포지토리에 앱을 설치하여 사용할 수 있으며, `.pre-commit-config.yaml` 파일만 있다면 별도의 설정이 필요하지 않습니다.

뛰어난 캐싱 성능 덕에 hook 실행속도가 다른 CI에 비해 매우 빠릅니다. 또한 주기적으로 설정파일 내에 있는 hook의 `최신버전을 검사`하여 봇이 PR을 날려주기 때문에 유지보수가 수월합니다.

![pre-commit-ci-example](/images/infra/pre-commit-ci-example.png#center)

CI가 동작되면 위와 같은 페이지에서 결과를 확인할 수 있습니다. 깃헙 레포지토리에서 활용가능한 `badge`를 제공하고 있기 때문에 사이트 접근 없이도 CI 결과를 확인해볼 수도 있습니다.

마치며
---

지금까지 `pre-commit`을 활용하여 쉽게 `Git Hooks`를 관리하는 방법에 대해서 알아보았습니다. 다음 포스팅에서는 `pre-commit hook`을 직접 만들어보고 배포하면서 겪은 일들에 대해서 공유해보도록 하겠습니다.
