---
title: "N Tier Architecture"
date: 2020-12-26T10:12:54Z
categories:
  - General
tags:
  - N-tier
  - General
draft: false
summary: >
  Multi-layer architecture discovered in a major class project
---


The multi-layer architecture is typically used in `client-server` applications. I think it is a concept that must be learned as it is a basic structure used in most applications, including web applications.

layer
---

There are three layers that make up the n-tier architecture:

- Presentation: User Interface (UI)
- Application (or logic): Handles business logic
- data: refers to database

The point to note here is that `tier` and `layer` are distinct concepts. Layer means physically separated, and layer is a logically separated concept.

2-tier vs 3-tier
---

![2-tier vs 3-tier](/images/architecture/n-tier-architecture/2-tier-vs-3-tier.jpeg#center)

### 2-tier

- Client program implemented through Java, etc.
- Expresses data obtained by directly connecting to the DB.

### 3-tier

- A typical example is a web application.
- Connect to middleware without directly connecting to the DB

For reference, the 2-tier structure may be implemented as a web application rather than a client program. Also, conversely, it is also possible to implement the client program as a 3-tier.

Implementation example
---

[University course registration system](https://github.com/junghoon-vans/Course-Enrollment-System), which I worked on as a major project during my second year of college, is a good example to explain the multi-layered structure. This program, implemented over two semesters, started with a 1-tier structure and evolved into a 3-tier structure. Below is a brief summary of the characteristics of each program version.

- v1.0
 - 1-tier Program
 -A simple client program that runs locally
 -Contains and loads data files related to course registration.
 -UI, business logic, and data all in one place
- v2.0
 - 2-tier Program
 -The client program simply has presentation functions.
 -Create a server program to handle client requests
 -The client expresses only the UI, and the server processes logic and data.
- v2.1+
 - 3-tier Program
 -Migrate data files to MySQL DB
 -The DB is deployed to `aws-rds`
 -Structure where all functions are physically separated

Looking back now, I think I didn't realize the true value of the project when I was working on it last year. I tried my best to create it according to the professor's class, but could I say that I didn't understand all the content contained in it? Why didn't I know earlier that the project that had been in progress for a year had a multi-layered evolution process?

In conclusion
---

Through this post, I learned that learning is not just about knowing new things. The review process of looking back at previously undertaken projects can also be a meaningful time. Of course, it would be best if you could understand everything when you first learn, but unfortunately, I don't think I can do that yet. I plan to write a blog post during my time in the military, organizing previously learned concepts and organizing new content to learn in the future.
