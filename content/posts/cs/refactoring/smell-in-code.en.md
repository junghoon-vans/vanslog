---
title: "Smell In Code"
translationKey: posts/cs/refactoring/smell-in-code
date: 2021-04-13T13:51:13Z
series:
  - General
categories:
  - General
tags:
  - General
draft: false
summary: >
  Code smell, code smell
---


There are no clear rules for when to refactor. However, there are characteristics of code that desperately need refactoring. Kent Beck defines this as `smell(bad smell)`.

strange name
---

Code should be written simply and clearly. In particular, the name should make it clear what the code does just by looking at it. These efforts will soon make the code easier to understand.

If you can't think of a good name, there may be a fundamental problem with the design. Organizing the names well can sometimes make the code concise.

duplicate code
---

If the same code structure is repeated, you can create a better program by combining it into one.

|Refactoring|explanation|
|----------|------|
|Extracting Functions|<li>For simple duplicate code</li> <li>Call a method extracted from duplicate code</li>|
|Slide a sentence|<li>In cases where they are not exactly the same but are similar</li> <li>Collect similar parts in one place to make it easier to apply `Extracting the function`</li>|
|Post method|<li>Duplicate code between child classes is moved to the parent</li>|

long function
---

|Refactoring|explanation|
|----------|------|
|short function|<li>The longer the function, the harder it is to understand</li> <li>The effect of indirect calls, making code easier to understand, share, and select</li>|
|good name|<li>Make the parts that need comments into functions</li> <li>The body of the function contains the code you wanted to explain with comments.</li> <li>The function name is named `intent`, not `method of operation`.</li>|

Long parameter list
---

The longer the parameters become, the more difficult it is to understand.

|Refactoring|explanation|
|----------|------|
|Converting parameters into query functions|<li>Remove parameters whose values ​​can be obtained from other parameters|
|Passing the entire object|<li>Pass to the original data structure|
|Create a parameter object|<li>Binding parameters that are always passed together|
|Remove flag argument|<li>Remove flags that control how functions operate.|
|Bundle multiple functions into a class|<li>Defining common values ​​as fields of a class</li> <li>Useful when multiple functions commonly use the values ​​of specific parameters</li>|

global data
---

The problem is that global data can be touched anywhere in the code base, and there is no mechanism to find out who changed the value.

|Refactoring|explanation|
|----------|------|
|Encapsulating Variables|<li>Wrap data in a function</li> <li>Easily find the part where data is modified</li><li>Access control possible</li>|

variable data
---

Changing data often leads to unexpected results or annoying bugs.

|Refactoring|explanation|
|----------|------|
|Encapsulating Variables|<li>Set the value to be changed only through a designated function</li> <li>Can monitor how the value is modified</li> <li>Easy to improve the code</li>|
|Split Variables|<li>Values ​​for different purposes are stored in independent variables for each purpose</li> <li>It is better to separate update logic from other code</li> <li>Use statement sliding and function splitting</li> <li>Separate code without side effects from code that updates something</li>|
|Separate query and change functions|<li>Prevent calling code with side effects|
|Removing the setter|<li>Helpful in reducing the effective range of variables</li>|
|Converting derived variables into query functions|<li>Variable data whose value can be set elsewhere stinks</li>|
|Bundle multiple functions into a class/transformation function|<li>Variables with a wide effective range have a high risk of causing problems</li> <li>Limit the effective range of codes that update variables</li>|
|Converting References to Values|<li>In the case of variables that contain data in internal fields, such as structures</li> <li>It is better to replace the entire structure rather than modify the internal fields directly</li>|

tangled change
---

The structure of software should be organized in a way that is easy to change. Otherwise, `Tangled Change` or `Shotgun Surgery` will appear.

Convoluted changes occur when `SRP (Single Responsibility Principle)` is not satisfied. In other words, it occurs when one module is changed a lot for different reasons.

Since business logic and database integration occur in different contexts, they should be configured as independent modules. However, it is not easy to divide these boundaries in the early stages of development, and these boundaries are constantly changing during the development process.

|Refactoring|explanation|
|----------|------|
|step splitting|<li>In the case of sequential contexts such as fetching and processing data from business logic</li> <li>Separate the steps by having the data required for the next context be delivered in a specific data structure</li>|
|Moving functions|<li>When functions from different contexts are frequently called throughout the entire processing process</li> <li>Create appropriate modules for each context and collect related functions</li>|
|Extracting Functions|<li>When moving a function, if there is a function involved in multiple contexts</li>|
|Extracting classes|<li>If the module is a class, separate it by context by extracting the class</li>|

shotgun surgery
---

Shotgun surgery is similar to, but also the exact opposite of, tangled change.

This smell arises when there are many classes that require minor modifications every time the code is changed. If the parts to be changed are spread throughout the code, it is difficult to find them and it is easy to miss the places that really need to be changed.

|Refactoring|explanation|
|----------|------|
|Moving functions / Moving fields|<li>Bundle the objects that change into one module</li>|
|Bundle multiple functions into a class|<li>When there are many functions that handle similar data</li>|
|Bundle multiple functions into a conversion function|<li>For functions that transform or augment data structures</li>|
|step splitting|<li>When passing the output results of functions grouped like this to the next stage of logic</li>|
|Inlining functions / Inlining classes|<li>A good way to deal with shotgun surgery caused by poorly separated logic</li> <li>Blended methods or classes, but improved with later extract refactoring</li>|

functional favoritism
---

A smell that arises when a function interacts more with other modules than with the functions or data of the module it belongs to.

|Refactoring|explanation|
|----------|------|
|Moving functions|<li>Close related functions and data|
|Extracting Functions|<li>If you prefer a function to only part of a function</li> <li>Extract that part and an independent function</li> <li>Move to the desired module with `Move function`</li>|

>If it's not clear where to move, move to the module that contains the most data.

bunch of data
---

A bundle of data that always moves together deserves a separate home.

|Refactoring|explanation|
|----------|------|
|Extracting classes|<li>Bundle of field-type data into one object</li>|
|Creating a parameter object / Passing the entire object|<li>For chunks of data in method signatures</li> <li>Apply this refactoring to reduce number of parameters</li>|

>Taking a class gives you the opportunity to spread a good scent.

basic obsession
---

There are many people who are obsessed with the basic types provided by programming languages ​​and are reluctant to directly define basic types appropriate for a given problem.

- Converting primitives to objects
 -It looks like it's just wrapping a basic data type.
 -Special actions can be added if necessary
- When code expressed as a basic type is used as a type code to control conditional operation
 -Convert type code to subclass
 -Converting conditional logic to polymorphism
 -Apply the above refactoring sequentially.

Repeated switch statement
---

It is better to eliminate switch statements as much as possible by changing conditional logic to polymorphism. This is because every time you add a conditional clause, you need to find all other switch statements and modify them as well.

repetition
---

Nowadays, many languages ​​​​support `first-class function`. Let's apply `Convert Loops to Pipelines` to remove loops that are not appropriate for the times. Using pipeline operations such as filters or maps makes it easy to understand how each element is processed in the code.

insincere elements
---

In fact, in the case of poor modules, such as classes with only one method, removal is performed. Processed as `inlining a function` or `inlining a class`. If inheritance is used, use `combining layers`.

speculative generalization
---

It comes from code that has all kinds of hooking points and special case handling logic that are not needed right now.

- Abstract class that does very little
 -Merge Layers
- Useless delegation code
 -Deleted by inlining a function or inlining a class
- Parameters not used in the body
 -Delete by replacing function declaration
- A function or class that is used nowhere else than in test code.
 -Removing dead code

temporary field
---

Sometimes there are classes that have fields whose values ​​are set only under certain circumstances. In these cases, users are left scratching their heads trying to figure out why a seemingly unused field exists.

1. If you find scattered fields, collect them with `Extract classes`.
2. Put the code related to temporary fields into the class created earlier using `Move Function`.
3. The conditional logic that operates after checking whether temporary fields are valid is `Add a special case`, which creates an alternative class for when the fields are invalid and removes them.


References
---

- [Martin Fowler, 『Refactoring 2nd Edition』, Hanbit Media](http://www.yes24.com/Product/Goods/89649360)
