---
title: "Tech Blog Reorganization"
date: 2022-07-13T15:35:56+09:00
draft: false
categories:
  - Blog
tags:
  - Blog
---
> Note: This English page scaffold was auto-generated. Full manual translation will follow.


I started a tech blog to organize what I learned. However, due to being busy during the semester, the cycle of uploading posts to the blog became longer. There are many cases where I just think about writing a blog post but am unable to do so.

I decided that I should not give up on running the blog like this, so I decided to calm down. We have completely reorganized the blog in the sense of starting it again with a fresh mind.

## New beginning

### Change blog theme

I first created this blog using `hugo` in November 2020. The reason I decided to use hugo at that time was because it was based on `golang`, so it was very fast and lightweight. Currently, `hugo` is the second most commonly used static site generator after `Next.js`. (see [Jamstack](https://jamstack.org/generators/)) 

So, I kept the blog based on hugo, but changed the theme of the blog with the intention of starting a new blog. The criterion for selecting the theme was `Simple Design`, which focuses on the content. Because I think the core of a blog lies in `posts`.

Then, a theme called [paper](https://github.com/nanxiaobei/hugo-paper) caught my eye. I really liked the minimalistic design, but it was lacking in functionality when actually using it. So, we applied [papermod](https://github.com/adityatelange/hugo-PaperMod), a theme that complements this.

### Migrate from Github Pages to Vercel

Previously, blog hosting was done through `Github Pages`. However, after experiencing several GitHub server errors recently, I felt that hosting my blog was unstable, and decided to migrate to another service.

I was debating between the two services, `netlify` and `Vercel`, but I found out that Vercel was superior in terms of domestic service speed, so I decided to use `Vercel`.Before distributing it, I had a vague idea that it would be difficult. This is because I had experience deploying the backend on AWS, but not the frontend. However, the actual deployment process was simple enough to just link to the GitHub repository. Through this, ‘Don’t be scared by guessing before you try’ was able to learn a lesson(?).

### Buy domain

I've wanted to purchase a domain for my personal blog for a long time, but I've been putting it off because of the price. While searching, I found out that a place called [DreamHost](https://www.dreamhost.com/domains/io/) was offering an io domain for $29.99 (for the first year only, $39.95 thereafter), so I purchased it.

I was able to easily register a custom domain through DNS settings in Vercel. Through this, I was able to experience purchasing and applying a domain.

## The cover is not important

`Blog` and `Notes` are not much different in that they are tools for recording. The difference between the two is whether the recording space is a digital world or an analog world.

In the analog world, if notes are not recorded, they lose their value and are discarded. It doesn't matter whether the cover is pretty or ugly. Likewise, I think blogs should `meaningless if not written down` even if they have a good design.

This blog reorganization was simply an act of refreshingly changing the outer cover of the notebook. I will strive to write consistently under the belief that the real value of a blog comes from its writing.