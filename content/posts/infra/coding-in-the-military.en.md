---
title: "Coding In The Military"
date: 2020-12-01
series:
  - General
categories:
  - General
tags:
  - General
draft: false
summary: >
 How can I code in the military? A journey to find the answer
---
> Note: This English page scaffold was auto-generated. Full manual translation will follow.


Any able-bodied male of Korean nationality must join the military, even if he or she does not want to. Not being able to continue what one is doing while away from society leads to a great loss to individuals in terms of career interruption. In particular, as the nature of the job of a developer requires one to react sensitively to and study rapidly changing trends, the resulting frustration is bound to be even greater.

Shortly after joining the military, I attempted to install and develop a development environment on my local PC, but due to the following constraints, I gave up on this method after using it a few times.

```plaintext
- ` (ftp, telnet)` .
- `git` .
- PC .
```

Let's look at the restrictions above again. You can see that there is no problem at all with `website access` using the http or https protocol. Therefore, communicating through a web browser does not violate the guidelines. I thought there was an answer here, and I figured out how to code in the military.

*If you build a web-based development environment, you can use a personalized development environment that is not initialized!*

As I found out through Googling, there were several developers who used `cloud9` or `cloudide` while serving in the military. However, I built a development environment using `Google Cloud Platform`. GCP offers basic instances free for life and a $300 credit that can be used for 3 months. This is a credit that allows you to continue operating an instance with Intel's quad-core CPU and 15GB of RAM memory for at least 3 months. In web/app development, you can build a server that provides sufficient performance.

> Previously, it offered a $300 credit that could be used for one year, but the period of use was shortened.

In future posts, I will introduce how to create a server and build a development environment. There are only two steps to prepare. Create a server and configure the development environment.

- Part 1 [Creating a GCP instance](../create-gcp-instance)
- Part 2 [Building a web IDE with __HOLD_5__](../build-web-ide-using-code-server)

The server does not necessarily have to be configured with GCP. You can use a personal server or another service, but this time I tried using GCP and summarized the method.