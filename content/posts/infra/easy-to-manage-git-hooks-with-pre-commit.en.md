---
title: "Easy To Manage Git Hooks With Pre Commit"
translationKey: posts/infra/easy-to-manage-git-hooks-with-pre-commit
date: 2022-10-12T00:15:32+09:00
draft: false
categories:
  - Infra
tags:
  - Pre-commit
  - Git Hooks
---

Git Hooks
---

`Git Hooks` is a script that runs when an event occurs in Git. It is used by writing a script in the `.git/hooks` directory, which allows you to check code conventions or run test code just before committing.

### Problem

If you are developing alone, this method may not be bad, but if several people participate in development, the following problems may occur.

1. Difficulty sharing hook scripts.
2. There is no guarantee that everyone uses the same version of the hook.

Apply pre-commit
---

[pre-commit](https://pre-commit.com) is a **good solution** that easily solves this problem. You can manage the version of hooks through a configuration file within the project, and easily install them on your local machine.

### pre-commit installation

```bash
$ pip install pre-commit  # install with pip
$ brew install pre-commit  # install with Homebrew 
```

### Create configuration file

Create a file named `.pre-commit-config.yaml` in the project root path. Afterwards, create a list of necessary hooks as follows.


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

> You can search for a list of supported hooks in [the site](https://pre-commit.com/hooks.html).

### Install hook script

The command below actually applies the hooks defined above to the `pre-commit` file in the `.git/hooks` path.

```bash
$ pre-commit install
```

Execute pre-commit
---

### Local machine

If you have completed all settings through the above process, the specified hooks will operate sequentially each time you commit. If you want to force it to run without committing, you can use the following command.

```bash
$ pre-commit run --all-files
```


![pre-commit-example](/images/infra/pre-commit-example.png#center)

The image above is a screen that runs while I was developing a Python package called [checkstyle-cli](https://github.com/junghoon-vans/checkstyle-cli). You can create a consistent source code style through the process of activating multiple `linter` or `formatter` for each commit.

### pre-commit.ci[pre-commit.ci](https://pre-commit.ci) is a service that runs `pre-commit hook` when source code is pushed to GitHub. You can install and use the app in the GitHub repository, and no separate settings are required as long as you have the `.pre-commit-config.yaml` file.

Thanks to excellent caching performance, hook execution speed is very fast compared to other CIs. In addition, maintenance is easy because the bot periodically sends out PRs by `check for latest version` of the hook in the configuration file.

![pre-commit-ci-example](/images/infra/pre-commit-ci-example.png#center)

Once CI operates, you can check the results on the page above. Because `badge` is available in the GitHub repository, you can check the CI results without accessing the site.

In conclusion
---

So far, we have learned how to easily manage `Git Hooks` using `pre-commit`. In the next post, I will share what I experienced while creating and distributing `pre-commit hook`.
