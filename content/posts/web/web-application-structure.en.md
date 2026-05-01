---
title: "Web Application Structure"
translationKey: posts/web/web-application-structure
date: 2020-12-26T14:06:55Z
categories:
  - Web
tags:
  - General
  - SSR
  - CSR
  - SPA
  - MPA
draft: false
summary: >
  We have compiled a list of terms that are trending recently in web front-end.
---


introduction
---

I think I have worked hard to pursue my dream of becoming a back-end developer. As a result, I served briefly at an industrial company called Ellis. But now that I think about it, I realized that I was missing a lot of knowledge about the overall web and front-end trends because I was only studying the back-end. After entering the military, I became interested in front-end development while creating a blog. However, this does not mean that I am now declaring that I will become a front-end developer. I came to the thought that I should not ignore the front end just because I was a back-end developer. So, through this article, I decided to study terms (SSR, CSR, SPA, MPA) that are recently trending in web front-end.

Static Page
---

A static page refers to a web page where files (HTML, JS) stored on a web server are delivered to the user as is. As long as the data is not directly changed, the page appears the same and has fast rendering speed. When creating a site that renders fixed content, such as a personal blog, static pages are often used. Usually, instead of creating a static site from scratch, `Static Site Generator` is used, and representative tools include `jekyll`, `hexo`, `gatsby`, and `Hugo`.

Dynamic Page
---

It literally refers to a dynamic web page. Unlike static pages, the same page can express different information depending on the data delivered by the application. For example, when accessing ‘Facebook’, the information displayed for each user is different. However, everyone is clearly aware that they are accessing the same page (www.facebook.com). The majority of modern web pages are created using this method.

### CSR(Clent-Side Rendering)

![csr](/images/web/web-application-structure/csr.png)

This refers to a method of rendering at the client side. Download and render everything necessary for rendering, such as HTML and JS, in the browser. Afterwards, only the necessary data is requested from the server and processed dynamically using JS.

### SSR(Server-Side Rendering)

![ssr](/images/web/web-application-structure/ssr.png)

This is how traditional web applications work. After rendering is completed on the server, the client receives it and executes it. Unlike `CSR`, refreshing occurs every time you move to another page, not just when you first access the page.

### SPA(Single Page Application)

It is an application consisting of one page. Sites created in this way only load the page the first time you access it, and then update only the necessary data by requesting from the server. So it has the characteristic of not refreshing the page. A representative example is 'React', a library that has recently become extremely popular in front-end development, and Facebook is a representative site created through it. In fact, when you use Facebook on the web, page loading occurs only the first time you access it, and no refresh process occurs at all thereafter.

### MPA(Single Page Application)

It refers to an application consisting of multiple pages and is the opposite of an SPA. The complete page is received from the server, and the browser needs to be refreshed to view other data.

SPA != CSR
---

If you read the above, you will see that SPA and CSR are similar concepts. However, you should not misunderstand that they are the same concept. To implement `SPA (Single Page Application)`, `CSR (Client-Side Rendering)` method is used, but it is not an equational relationship. If we were to be specific, we could say that ‘one of the elements that make up SPA is CSR (SPA⊃CSR)’.

> The relationship between MPA/SSR is the same as that of SPA/CSR. SSR is used to implement MPA.

SPA vs MPA
---

| Category | SPA | MPAs |
|------|-----|-----|
| Advantages | <ul><li>Component reuse</li><li>Flexible UI</li></ul> | <ul><li>Good for SEO</li></ul> |
| Disadvantages | <ul><li>Initial page loading time</li><li>Unfavorable to SEO</li></ul> | <ul><li>Refresh when moving to another page</li></ul> |

Search Engine Optimization Issues
---

`SEO (Search Engine Optimization)` refers to the process of ensuring that a product appears at the top of search results on a search engine. Search engines usually show the results of crawling websites, but most search engine crawlers except Google cannot interpret JS. `MPA` does not cause SEO problems because it displays pages that have already been rendered on the server, while `SPA`, which renders data through JS after `loading an empty page`, does not allow search engines to properly crawl it.

conclusion
---

The web pages we use today have various structures. Traditional methods include a 'static page' that simply displays resources located on the web server, and a method that provides pages rendered by the server through 'JSP/Servlet', etc. However, recently, 'SPA' was born due to the need for a 'web application that operates like an app' according to various requirements. Due to this change in paradigm, they began to be classified into `SSR` and `CSR` depending on the location where rendering takes place.

> Actually, if you think about it, the concept of CSR is not new at all. Even with traditional `MPA`, there were cases where some page contents were changed to asynchronous communication through `Ajax`.

Recently, SSR has been introduced to SPA to solve SEO problems. A representative example is `next.js`, a React-based framework. Using this, it is possible to support not only CSR but also SSR. This method of use is called ‘hybrid’. If you want to optimize search engine while maintaining SPA, you can use `react-helmet` or `react-snap`.

As a result, what I want to say is that there are various ways to implement web applications. There is no way to say which is the right answer, and you can use a mixture of various methods. In the end, I think it is important to solve the given task appropriately. If search engine optimization must be prioritized, `SSR` should be introduced, and if you want to provide a flexible UI and app-like user experience without refresh on the web, `CSR`-based `SPA` should be introduced.
