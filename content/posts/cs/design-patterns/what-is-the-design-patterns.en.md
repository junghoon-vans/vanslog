---
title: "What Is The Design Patterns"
date: 2021-01-16T10:52:57+09:00
categories:
  - Design Patterns
tags:
  - Design Patterns
  - OOP
  - Java
  - GoF
draft: false
summary: >
  Design patterns for good object-oriented design
---


Good object-oriented design
---

Designing `object-oriented software` is not easy. The following elements are essential for a good design.

- Maintainability
- Function expansion

Software may have `changes in requirements` even during the development stage, and `feature expansion` or `maintenance` are constantly occurring during service. Therefore, software must provide the flexibility to be reused without redesign.

###History repeats itself.

As you program, you encounter similar problems repeatedly and realize that methods to solve them are also used repeatedly. If you encounter a problem you've already experienced again, you can solve it relatively easily based on your previous experience. From this, we can see that `experience` is important in solving `repeated design problems`.

So experts have compiled solutions tailored to the situation they have accumulated through experience under the name `Design Pattern`. These repeating patterns solve specific design problems and create flexible, reusable object-oriented software.

>Design patterns: Excellent solutions that can be reused when chronic problems that frequently occur in a specific context arise again when designing software.

`Christopher Alexander (1977/79)`, architect and father of design patterns, explains patterns as follows:

*"Each pattern describes a problem which occurs over and over again in our environment, and then describes the core of the solution to that problem, in such a way that you can use this. solution a million times over, without ever doing it the same way twice."*

###Don't reinvent the wheel

`Don't reinvent the wheel` is a maxim that explains design patterns well. This means that there is no need to recreate something that has already been created and is working well from scratch. In other words, it emphasizes that you should not start from scratch unnecessarily.

By learning `design patterns`, which has been proven over decades, we don't have to put in the effort to find new solutions.

GoF design patterns
---

Design patterns have gained enormous popularity since `GoF(Gang of Four)` introduced them in `Design Patterns: Elements of Reusable Object-Oriented Software`. There are a total of 23 patterns introduced by them, and they are classified into `Generation Patterns`, `Structural Patterns`, and `Behavior Patterns` depending on the purpose.

###Classification according to purpose
####generative pattern

- `Abstract Factory`
 -Provides an interface to create a set of objects without specifying a specific class.
- `Builder`
 -By separating the creation process and expression method of a complex object, it is possible to produce different results from the same creation procedure.
- `Factory Method`
 -The interface for creating objects is defined in advance, but the class for creating instances is determined in the subclass.
- `Prototype`
 -Define a prototype to specify the type of object to be created, and create a new object by copying it.
- `Singleton`
 -Ensures that there is only one instance of a class and provides a global point of contact for accessing this object.

####structural pattern

- `Adapter`
 -Addressing compatibility by converting a class's interface to what the user expects.
- `Bridge`
 -By separating the abstraction layer from the implementation, each layer can be modified independently.
- `Composite`
 -The relationship between objects is organized into a tree structure to express the part-whole hierarchy.
- `Decorator`
 -A pattern that attaches responsibilities to an object based on a given situation and use.
- `Facade`
 -Provides a single, integrated interface for a set of interfaces in a subsystem.
- `Flyweight`
 -When there are multiple small objects, they are supported efficiently through sharing.
- `Proxy`
 -Provides a delegate or filler to control access to an object.

####behavioral patterns

- `Chain of Responsiblity`
 -Avoid coupling between objects sending and receiving requests by giving more than one object the opportunity to process the request.
- `Command`
 -Encapsulates requests in the form of objects to support parameterization, request storage, logging, and operation cancellation for different users.
- `Interpreter`
 -For a given language, defines the expressive means for the grammar of that language. Definition of an interpreter that interprets the document.
- `Iterator`
 -Sequential access to elements belonging to an object set without exposing the internal representation.
- `Mediator`
 -An object definition that encapsulates the interactions of objects belonging to a set.
- `Memento`
 -Captures and instantiates the internal state of an object without violating encapsulation, so that the object can later return to that state.
- `Observer`
 -By defining one-to-many dependency relationships between objects, when the state of an object changes, other objects that depend on that object can be notified of the change and automatically update themselves.
- `State`
 -Allows an object to change its behavior based on its internal state.
- `Strategy`
 -A pattern that defines a family of identical algorithms and encapsulates each algorithm to make them interchangeable.
- `Template Method`
 -For object operations, only the framework of the algorithm is defined and the specific processing to be performed at each step is postponed to the subclass.
- `Visitor`
 -A pattern that expresses the operation to be performed on the elements that make up the object structure.

###Classification according to scope

- class pattern
- Pattern for handling `relevance` between classes and subclasses
- Relevance mainly refers to `inheritance`
- `static` at compile time
- object pattern
 -Dealing with object relatedness
- `Dynamic` at runtime

|classification|generation|structure|action|
|:--:|:--:|:--:|:--:|
|class|factory method|Adapter (class)|Interpreter<br>Template Method|
|object|Abstract Factory<br>Builder<br>Circular<br>Solid|Adapter (object)<br>Bridge<br>Complex<br>Decoerator<br>Facade<br>Flyweight<br>Proxy|Chain of Responsibility<br>Command<br>Interpreter<br>Arbiter<br>Memento<br>Watcher<br>State<br>Strategy<br>Visitor|


###Design pattern relationship diagram

![Relationship diagram](https://www.cs.unc.edu/~stotts/GOF/hires/Pictures/bigmap.gif)

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
- [Eric Gamma and 3 others, 『GoF Design Patterns』, Protech Media](http://www.yes24.com/Product/Goods/17525598)
- [UNC Department of Computer Science, GOF](https://www.cs.unc.edu/~stotts/GOF/hires/contfso.htm)
