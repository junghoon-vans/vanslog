---
title: "Apply Utterances Dynamic Theme"
translationKey: posts/general/apply-utterances-dynamic-theme
date: 2022-07-17T15:12:13+09:00
draft: false
categories:
  - Blog
tags:
  - Utterances
  - General
  - Blog
---

`utterances` is an open source comment service widely used on personal technology blogs. It features a clean design of `GitHub style` and support for `dark mode`.

Recently, there are more and more sites that allow you to change night mode and dark mode to `Dynamic`. My blog is also like this, so I had to dynamically apply the theme of `utterances`.

When I googled it, I found very few mentions of this method. So, in this post, I would like to focus on how to dynamically apply the theme of utterances.

## How to apply Utterances

### Apply a single theme

When a website uses only one theme, you can use the comment function in the general way as follows.

1. Set the repo where you want to apply the comment function to `public`
2. Install [utterances app](https://github.com/apps/utterances) on GitHub
3. Access the [utterances](https://utteranc.es) site
4. Specify `repo` and `theme` and copy the script located at the bottom.
5. Paste the code where you want to place the comment

> When installing the utterances app, you only need to specify all repos or only the repositories for which you need comments functionality.

### Apply dynamic theme

For sites that support both dark mode and night mode, `Utterances` must also change the theme to match this. Otherwise, design unity will be lost.

#### Select location to apply comments

```html
/layouts/partials/comments.html

{{- /* Comments area start */ -}}
<div class="comments">
 <script>
 <!-- -->
 </script>
</div>
{{- /* Comments area end */ -}}

```

First, find the part where you want to add a comment. In the case of Hugo used in my blog, you can use it by overriding the theme. Those who create themes also take these things into consideration and create files in advance so that they can be overridden. So, I applied the comment function by overriding only the file called `comments.html`.

#### Write JavaScript code

```js
loadComment();
const callback = (mutationsList) => {
 mutationsList.forEach(mutation => {
 if (mutation.attributeName === "class" && document.querySelector('.utterances-frame')) {
 const message = {
 type: 'set-theme',
 theme: getTheme()
 };
 const iframe = document.querySelector('.utterances-frame');
 iframe.contentWindow.postMessage(message, 'https://utteranc.es');
 }
 })
}
const mutationObserver = new MutationObserver(callback);
mutationObserver.observe(document.body, { attributes: true });

function getTheme() { //
 var theme = window.localStorage && window.localStorage.getItem("pref-theme");
 if (theme == null) {
 theme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
 }
 return theme === 'dark' ? 'GitHub-dark' : 'GitHub-light';
}

function loadComment() {
 let s = document.createElement('script');
 s.src = 'https://utteranc.es/client.js';
 s.setAttribute('repo', 'junghoon-vans/vanslog'); //
 s.setAttribute('issue-term', 'pathname');
 s.setAttribute('theme', getTheme());
 s.setAttribute('crossorigin', 'anonymous');
 s.setAttribute('async', '');
 document.querySelector('div.comments').innerHTML = '';
 document.querySelector('div.comments').appendChild(s);
}
```The above code uses the `loadComments` method to load `utterances` comments, and then `mutationObserver` recognizes changes in the state of the DOM and changes the theme of the utterances. The `getTheme` method determines what mode the site is currently in and specifies the theme to apply to the comment.

My blog theme, `papermod`, saves information about the current mode in local storage under the name `pref-theme`. So, through this, we checked whether we were currently in dark mode or light mode. Also, if you have never specified a theme directly, the value in local storage may be `null`, so in this case, `System Settings` is automatically followed.

> If anyone wants to refer to this article and apply it to their own blog, please keep the following in mind. In the case of the `getTheme` method, the way to specify the color mode may vary depending on the site, so be sure to check which variable specifies the mode. Second, you must change the repository designation in the `loadCommant` method to your own.

## Conclusion

In the process of customizing my own blog, I got to touch the front-end source code. Before, I thought my area was limited to the back-end, but when I actually tried it, I realized that the front-end was also fun. I think it gave me an opportunity to think about whether I was setting my own limits. From now on, I have come to think that I need to have a more open mind and a mindset of studying in many different ways.
