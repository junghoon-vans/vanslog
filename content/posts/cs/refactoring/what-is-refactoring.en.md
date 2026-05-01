---
title: "What Is Refactoring"
date: 2021-02-06T02:34:16Z
series:
  - General
categories:
  - General
tags:
  - General
draft: false
summary: >
  Code restructuring and refactoring
---


>This article was written with reference to Martin Fowler's `Refactoring 2nd Edition`.

Martin Fowler's Definition
---

The term `refactoring` is widely used among engineers. Martin Fowler, author of Refactoring, defines refactoring as follows.

- [noun] A technique of changing the internal structure of the software to make it easier to understand and modify the code while maintaining the apparent behavior of the software.
- [verb] To reorganize software by applying various refactoring techniques while maintaining the apparent behavior of the software.

>Here, the meaning of the expression observable behavior is that the code behavior before and after refactoring is the same.

Many people express the task of organizing code as `refactoring`, and according to the definition presented above, refactoring only involves organizing code according to a specific method.

Refactoring involves numerous small steps sequentially creating changes, and during this process, the code must always work. If someone says they suffered for days because their code broke while refactoring, this is not refactoring in the true sense of the word.

two hats
---

When developing software, `adding features` and `refactoring` must be clearly distinguished depending on the purpose. Kent Beck liked this method to `Two Hats`.

>Kent Beck is one of the pioneers of refactoring and helped Martin Fowler, author of `refactoring`, write the article.

- When adding features, wear the `add features` hat and add new features.
- When refactoring, I put on my `refactoring` hat and focus only on reorganizing the code.

While adding features, if you feel that it would be easier to work if you change the structure, change hats for a while and refactor. Once the code structure has been improved to some extent, change hats again and continue adding features.

In this process, I must be clearly aware of the hat I am currently wearing and the differences in work methods accordingly.

Benefits of Refactoring
---

- good software design
 -If you only implement functions without understanding the architecture, the infrastructure will collapse.
 -Ultimately, it becomes difficult to understand the design when looking at the code later.
 -Refactoring prevents design corruption
- Easy to understand software
 -Source code isn't just for computers to read
 -Refactoring makes code more readable
 -Make the purpose of your code more evident
 -Make your intentions clearer
- Easy to find bugs
 -Making code easy to understand means making it easy to find bugs.
 -A clear program structure makes it impossible to overlook bugs.
- Accelerate programming speed
 -It is easy to misunderstand that refactoring slows down development.
 -Good design provides a solid foundation to help build new features
 -Therefore, refactoring is essential for rapid development.

When to Refactor
---

###Refactoring for Preparation

- Right before adding a new feature to the code base
- Look for areas where changing the structure would make other tasks much easier.

###Refactoring for Understanding: Making Code Easier to Understand

- First figure out what the code does
- Look for room for refactoring to make the intent clearer.
- Comprehension Refactoring
 -Change variables to appropriate names
 -Breaking down long functions into smaller pieces

###Garbage picking refactoring

- inefficient code
 -complex logic
 -Write multiple functions to do what could be done with a parameterized function
- Take your time and improve little by little
 -Divide the work

###Planned refactoring and frequent refactoring

- Refactoring is not a separate activity from programming.
 -This is like not setting aside time to write if statements.
 -Do it while you're doing something else
- Well-written code also needs to be refactored.
- Not that planned refactoring is a bad thing, but keep it to a minimum.
 -Go ahead whenever you get the chance

###Refactoring takes a long time

- massive refactoring
 -Library replacement
 -Componentize some code to share it with other teams
 -Dependency cleanup
- It's not a good idea for the whole team to get hung up on problems like the above.
- It is more effective to solve the problem over a period of several weeks.
 -The person working on the code to be refactored improves it little by little.
 -Provide an abstract interface when changing libraries
 -Make existing code call this abstract interface
- This strategy is called `Branch By Abstraction`

###Used for code review

- Benefits of Code Reviews
 -Spread knowledge across the development team
 -Transfer of know-how from senior developers
 -understand different aspects
 -Write clean code
 -Get ideas from others
- Deriving more specific results from code reviews
 -Rather than just suggesting improvements, you can implement many of them yourself.
 -You might come up with a better idea

###When not to refactor

- When there is no need to modify
- When it's better to write from scratch

consideration
---

###Slow development of new features

Although many people believe that refactoring slows down the development of new features, the ultimate goal of refactoring is actually to speed up development. There are still many people who believe that refactoring slows down development, so refactoring is not applied properly in practice.

However, you should not make mistakes about refactoring. Refactoring is justified for moral reasons, such as `Clean Code` or `good engineering practice`. The essence of refactoring is not to make the code base look pretty. This is done solely for economic reasons (shortening the development period).

###Code Ownership

Refactoring often affects not only the internals of the module, but also the way it interacts with other parts of the system. For example, when you try to change the name of a function, the owner of the calling code may be `another team`, so you may not have write permission. Or, if it is a function provided as `API`, you may not even know if it is actually used, let alone how much it is used.

Divided code ownership hinders refactoring. This is because it cannot be changed to the desired form without affecting the client. That doesn't mean there isn't a way. When changing a function name, the existing function is kept as is, but the new function is called from the function body. Existing functions are designated as disposal targets and deleted over time.

The best way is to give ownership of the code to the team. So, any team member can modify the code owned by the team. This loose approach to code ownership also applies to organizations comprised of multiple teams. For each team, you can follow `Branch` and use `Open Source Development Model` to request a commit. This way, you can also change the client of the function.

###branch

Typically, teams work in branches and integrate using a version control system. Using this method, the version can be clarified and rollback is possible easily if there is a problem with the function. The downside to this method is that the longer you work on independent branches, the more difficult it becomes to integrate the results of your work into master.

>Conflicts can easily occur when changes occur between branches.

Therefore, to solve this problem, you must `rebase` or `merge` from time to time. Shortening the branch integration cycle is called `Continuous Integration (CI)` or `Trunk-Based Development`. Refactoring and CI shine when used together. Kent Beck invented `Extreme Programming (XP)` as a development method that combines refactoring and CI.

###testing

The biggest characteristic of refactoring is that even if the internal structure changes, the apparent behavior remains the same. Even if you make a mistake, you can just go back to the previous version that worked properly.

The key here is to catch errors quickly, which requires `Test Suite`. You must be able to execute this quickly so that there is no burden of frequent testing. In other words, `self-testing code` is required for refactoring.

Test code not only helps with refactoring, but also allows new features to be added safely. This is because you can quickly find and eliminate accidentally created bugs. It is very important to have solid tests to support this.

Self-test code can be used as a mechanism to catch semantic conflicts that occur during the integration process, so it is naturally closely related to CI. Testing integrated into CI is a recommendation in XP and also the core of `Continuous Delivery (CD)`.

###legacy code

`Legacy Code` is generally complex and often lacks proper testing. Refactoring can be helpful in understanding this legacy code. Adding tests here allows for clearer refactoring.

The best approach is to test every part of the system by finding niches to add tests to, but testing all of the vast swaths of legacy code can be difficult. It is effective to refactor more by focusing on frequently viewed parts. The more times you scan the code, the greater the effect you get when you improve that part to make it easier to understand.

###database

In the past, it was difficult to refactor databases, but with the advent of `evolutionary database design` and `database refactoring` techniques, this has become a myth. The key to this technique is to write `migration script` and integrate access code and structural changes to the database schema into this script.

>Database migration used in ORM is a representative example.

Like other refactorings, this technique breaks the overall change process into small, independent steps. Only then will it be able to operate normally even after migration.

Database refactoring differs from other refactorings in that it is generally better to divide it into several stages and release it depending on the production environment. This makes it easier to revert changes if problems arise in the production environment.

For example, when changing a field name, the first commit only adds a new database field and does not use it. Then, set the existing and new fields to be updated simultaneously. Next, clients that read the database are gradually replaced with versions that use the new fields. Once you have completed all client replacement tasks while resolving any bugs that arise during this process, delete the old fields that are no longer needed.

>This way of changing the database is a common example of `parallel change` or `expand contract`.

YAGNI
---

Refactoring allows you to drastically change the architecture of software that has been in operation for several years. The practical effect of refactoring on architecture is that it helps design the code base to naturally respond to `changes in requirements`.

Another method to flexibly respond to requirements changes is `flexibility mechanism`. For example, to use the function universally, parameters corresponding to the expected scenario are added. At this time, the parameter is a flexibility mechanism. However, if you take all situations into consideration, there are cases where you actually reduce your ability to respond to change. This is because requirements may change during development.

You can approach it differently by using `refactoring`. First, build software that satisfies the requirements identified to date. As development progresses and we gain a better understanding of the requirements, we refactor the architecture accordingly. This design method is called `Concise Design`, `Incremental Design`, and `YAGNI(you aren't going to need it)`.

>While a flexibility mechanism is implemented by anticipating requirements, refactoring is a method of continuously expanding upon identified requirements.

YAGNI is another way to incorporate architecture and design into the development process. This does not mean that you should neglect architecture. Thinking ahead may save you time, but it may be much better to deal with it later when you have a deeper understanding of the problem. This trend led to the development of the `evolutionary architecture` principle.

software development process
---

- Extreme Programming (XP)
 -Continuous Integration (CI) + Refactoring
 -The first agile software methodology
- Test Driven Development (TDD)
 -Self-testing code + refactoring
- 3 elements for practicing YAGNI
 -self test code
 -continuous integration
 -Refactoring
- Continuous Delivery (CD)
 -Keep software ready to be released at any time

Refactoring and Performance
---

`Intuitive design vs performance` is an important topic. Refactoring may make the software slower, but it also makes performance easier to tune.

###How to Write Software Fast

- Time budget distribution method
 -Divide the design into multiple components
 -Allocate resource (time, space) budget to each component
 -strict punctuality
- constant attention
 -intuitive
 -The actual effect is insignificant
 -Spend most of your time on a small portion of your code
 -90% of all code has little optimization effect
- Optimize only the parts that affect performance
 -Don't worry about performance, just focus on code structure
 -When you reach the performance optimization stage, follow these steps:
 -Use the profiler to find spots that consume a lot of time and space
 -Improve performance on that point
 -Take small steps, like refactoring
 -Compile/test at each step

###Why refactoring is beneficial for performance optimization

- Buy time for performance tuning
 -Saves time adding features
 -Improve performance in your spare time
- Performance can be analyzed in detail
 -Profiler points to narrow code coverage
 -So easy to tune

References
---

- [Martin Fowler, 『Refactoring 2nd Edition』, Hanbit Media](http://www.yes24.com/Product/Goods/89649360)
