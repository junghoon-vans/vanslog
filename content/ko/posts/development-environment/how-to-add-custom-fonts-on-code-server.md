---
title: "코드 서버에 커스텀 폰트 적용하기"
date: 2021-03-11T14:24:16Z
categories:
- 개발 환경
tags:
- 개발 환경
- 코드 서버
draft: false
description: >
    코드 서버에 커스텀 폰트 적용하기
---

`코드 서버`를 개인 서버에서 운용하게 되면서 언제 어디서든 인터넷만 가능하면 코딩할 수 있고 블로그 포스팅도 가능한 환경을 구성할 수 있었다.

다만 하나 아쉬웠던 거라고 하면 손쉽게 원하는 폰트를 사용할 수 없다는 점이었다. 로컬에서 `VS Code`로 작업할 때처럼 간단하게 설정해서 원하는 폰트를 사용할 수 있다면 편리하겠지만 그렇게 쉬운 일이 아니었다.

만약에 `코드 서버`에 접속할 PC가 모두 내가 사용하는 폰트가 설치되어 있다면 정상적으로 출력이 될 수 있다. 하지만 이런 환경이 항상 보장되는 것은 아니다. 우리가 `코드 서버`를 사용하는 이유는 언제 어디서나 동일한 개발 환경을 제공받기 위해서이다. 하지만 매번 폰트를 설치해야 한다는 이러한 취지에 위반된다고 생각했다.

해결 방법
---

이를 해결하는 방법으로는 `코드 서버`에 폰트도 같이 호스팅하는 것이다. 이것은 간단한 소스 코드 수정으로 가능하다.

우선 `/usr/lib/code-server/src/browser/pages` 경로에서 `vscode.html` 파일에 아래와 같은 코드를 추가한다.

```html
<head>
...
<link rel="stylesheet" type="text/css" href="{{CS_STATIC_BASE}}/src/browser/pages/fonts.css">
...
</head>
```

이후 동일한 경로에 `fonts.css` 파일을 생성하고 아래와 비슷한 방식으로 폰트를 설정한다. 폰트는 `/usr/lib/code-server/src/browser/media`에 담아두었다.

> 평소에도 D2Coding-ligature를 즐겨 사용했기에 해당 폰트를 사용할 수 있도록 설정하였다.

```html
@font-face {
    font-family: 'D2Coding ligature';
    src: url('../media/D2Coding-ligature.woff2') format('woff2'),
        url('../media/D2Coding-ligature.woff') format('woff');
    font-weight: 400;
    font-style: normal;
}
```

여기까지 완료했다면 `VS Code`에서 하던 것처럼 폰트를 설정해주고 `코드 서버`를 재시동해주면 끝이다.

```bash
sudo systemctl restart code-server@$USER
```

문제점
---

해당 방법을 사용해서 폰트를 적용하면 `코드 서버` 업데이트 시 적용이 풀리는 현상이 있다. 해당 문제를 해결하는 방법을 찾아서 추후 소개하도록 하겠다.