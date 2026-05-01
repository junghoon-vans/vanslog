---
title: "How To Add Custom Fonts On Code Server"
date: 2021-03-11T14:24:16Z
categories:
  - General
tags:
  - General
  - Code-server
draft: false
summary: >
 Applying custom fonts to code server
---
> Note: This English page scaffold was auto-generated. Full manual translation will follow.


By operating `Code Server` on a personal server, we were able to create an environment in which coding and blog posting were possible anytime, anywhere as long as the Internet was available.

The only thing that was disappointing was that I couldn't easily use the font I wanted. It would be convenient if you could use the desired font by simply setting it up like when working with `VS Code` locally, but it was not that easy.

If all PCs connected to `Code Server` have the fonts I use installed, the output can be done normally. However, this environment is not always guaranteed. The reason we use `code server` is to provide the same development environment anytime, anywhere. However, I thought it violated the purpose of having to install fonts every time.

How to solve
---

A way to solve this problem is to also host the font in `Code Server`. This is possible with simple source code modification.

First, add the following code to the `vscode.html` file in the `/usr/lib/code-server/src/browser/pages` path.

```html
<head>
...
<link rel="stylesheet" type="text/css" href="{{CS_STATIC_BASE}}/src/browser/pages/fonts.css">
...
</head>
```

Afterwards, create a `fonts.css` file in the same path and set the font in a similar way as below. The font is stored in `/usr/lib/code-server/src/browser/media`.

> Since I usually use D2Coding-ligature, I set it up so that I can use that font.

```html
@font-face {
 font-family: 'D2Coding ligature';
 src: url('../media/D2Coding-ligature.woff2') format('woff2'),
 url('../media/D2Coding-ligature.woff') format('woff');
 font-weight: 400;
 font-style: normal;
}
```

If you have completed up to this point, set the font as you did in `VS Code` and restart `Code Server`.

```bash
sudo systemctl restart code-server@$USER
```

problem
---

If you apply the font using this method, the application may be released when updating `Code Server`. We will find a way to solve this problem and introduce it later.