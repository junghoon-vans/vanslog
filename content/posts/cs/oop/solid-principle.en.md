---
title: "Solid Principle"
translationKey: posts/cs/oop/solid-principle
date: 2021-01-10T19:53:10+09:00
series:
  - OOP
categories:
  - OOP
tags:
  - OOP
  - JAVA
draft: false
summary: >
  Object-oriented design principles
---


Single Responsibility Principle (SRP)
---

>`Single Responsiblity Principle (SRP)`: The principle of having only `Single Responsiblity`

###meaning of responsibility

The basic unit of responsibility in SRP refers to `object`. Therefore, it means that *an object should have only one responsibility*.

So what is ‘responsibility’? Usually responsibilities are considered `what you should do` or `what you can do`. And when assigning responsibility to an object, you should assign responsibility to the `can do well` object rather than any other object. Additionally, the object must perform all tasks that accompany its responsibility.

For example, let's assume that the `student` class adds or searches courses, stores and retrieves object information from the DB, and even prints them on report cards and attendance sheets.

```java
public class Student {
    public void getCourses() { ... }
    public void addCourse(Course c) { ... }

    public void save() { ... }
    public Student load() { ... }

    public void printOnReportCard() { ... }
    public void printOnAttendanceBook() { ... }
}
```

In this case, the `student` class has to carry too many responsibilities. In order to satisfy `Single Responsibility Principle (SRP)`, only the responsibilities that the `student` class can do best (add/check courses) are left. There is a lot of room for other classes to do well in DB work or report card/attendance printing.

###change

In order to create an effective design that follows SRP, responsibility needs to be understood as a more realistic concept. The reason we learn design principles is to design ‘System structure that is flexible and scalable to unexpected changes’.

Good design basically requires reducing the impact as much as possible when there are new requirements or changes to the system. For example, to determine whether a class is well designed, it is good to ask whether it should be `when to change`.

So when should the `student` class be changed?

- When changing DB schema?
- What if a function for students to find an advisor was added?
- What if you want to print in a new format?

These are all reasons to change student classes. Additionally, the more responsibilities there are, the more likely it is that code performing different roles within the class will be strongly coupled.

###separation of responsibilities

The `student` class performs multiple responsibilities, so there is bound to be a lot of code that needs its help. Therefore, if any changes are made to the `Student` class, all directly or indirectly related code must be retested.

>For reference, the test that evaluates whether changes affect existing system functions is called `regression test`.

In order to avoid testing all of your code, you should not assign too much responsibility to one class and only perform `only one responsibility`, thereby consolidating all possible reasons for change. This is called `separation of responsibility`.

![advanced-design](/images/oop/solid-principle/advanced-design.jpg#center)

In the case of the `Student` class, there are three possible reasons for change: changes in the student's `Proprietary Information`, `DB Schema`, and `Output Format`. Therefore, it is better to have the student class perform only its own role and separate the DB work into the DAO (Data Access Object) class and the class responsible for printing the attendance list and report card.

###shotgun surgery

So far we have looked at situations where a class has multiple responsibilities. Conversely, even in the case of `single responsibility spread across multiple classes`, the design must be changed based on the single responsibility principle. This case is called `shotgun surgery`.

An example of one responsibility being separated into multiple classes is a function that can be classified into `cross-cutting concerns`, such as `logging, security, transactions`. Most functions of cross-cutting interest are `additional functions`, which are contained within `system core functions`.

![cross-cutting-concern](/images/oop/solid-principle/cross-cutting-concern.jpg#center)

For example, if there is a code that stores the execution log of specific messages executed in the system in the DB, if you change it to save it as a file, you must find all methods in which the log function is inserted.

The way to solve this is to separate these additional functions into `separate classes` and make it responsible for them. In other words, `cohesion` is raised by gathering `common responsibility` scattered in various places in one place. However, the code that calls and uses the implemented functions must still be included somewhere in the code that uses the functions.

There is a technique called `Interest Oriented Programming(AOP)` as a way to solve the cross-cutting interest problem. AOP modularizes code that performs cross-cutting concerns into a special object called `aspect`, and inserts the modularized code into core functions through an operation called `weaving`. If there is a change in the traversing interest, only the relevant aspect is modified without changing the existing code at all.

Open-Closed Principle (OCP)
---

>`Open-Closed Principle (OCP)`: The principle that the design should allow functions to be added without changing the existing code.

![case-that-violate-ocp](/images/oop/solid-principle/case-that-violate-ocp.jpg#center)

What if we wanted to add the ability to print student rental records in a new medium, such as a library rental directory? It looks like the `Client` class can use this function by creating a new `Library Rental Directory` class. However, this method violates OCP. This is because the client must be modified as new features are added.

![case-that-satisfy-ocp](/images/oop/solid-principle/case-that-satisfy-ocp.jpg#center)

To ensure that adding a new function does not affect the `Client` class, you must process the specific function by `encapsulation` through `interface` rather than directly accessing `individual class`.

*The class must be designed so that the environment of the target class can be changed (open) without being changed (closed).*

Liskov Substitution Principle (LSP)
---

>`Liskov Substitution Principle (LSP)`: The principle that a child class must be able to perform at least the actions that its parent class can perform.

The generalization relationship is also called the `is-a-kind-of` relationship. For example, a corresponding relationship exists between monkeys and mammals (monkey is a kind of mammal). At this time, you can set `Mammal` as the parent class and `Monkey` as the child class.

```plaintext
- .
- .
- .
```

The above explains various characteristics of mammals.

```plaintext
- .
- .
- .
```

`Liskov Substitution Principle` allows a child class to perform actions that are possible in the parent class. Therefore, the characteristics of mammals can also be performed by monkeys.

###Is the platypus a mammal?

```plaintext
- .
- .
- .
```

Let's apply this same situation to `Platypus`. Although the platypus is a mammal, it is an animal that lays eggs rather than giving birth to young ones. In this case, actions possible in the parent class cannot be performed. Therefore, we can conclude `the description of mammals is wrong` above.

*To satisfy LSP, `instance of parent class` must be replaced by `instance of child class`.*

>In reality, `viviparous` is not used as a standard for classifying mammals. For reference, mammals that lay eggs are called `monotremes`. Therefore, the proposition ‘Mammals except monotremes are viviparous’ is true.

###overriding

But you may also have these questions. Since only a small number of mammals lay eggs, shouldn't `override` be used only in exceptional cases?

```plaintext
- . (override)
- .
- .
```

If you use redefinition, Platypus can be defined as above, and the code can actually work fine. However, two OOP rule violations occur as follows.

- Does not satisfy LSP
 -The implementation of the Platypus class is inconsistent with the behavior of the Mammal class.
- Peter Codd's violation of inheritance rules
- A rule called `The subclass does not override or override the responsibilities of the superclass, only extends it`

In Peter Code's inheritance rules, `only extend, not redefine` means the same thing as not overriding a word. Therefore, following Peter Code's inheritance rules is one way to satisfy LSP. **Do Not Override!**

Dependency Inversion Principle (DIP)
---

>`Dependency Inversion Principle (DIP)`: The principle of relying on things that are difficult to change or that rarely change when forming a dependency relationship.

Let's assume a person drinks a beverage. We drink water every day, but we also drink coffee or enjoy cola. What specifically you drink is easy to change, but the fact of drinking something itself is difficult to change.

![apply-dip-to-beverage](/images/oop/solid-principle/apply-dip-to-beverage.jpg#center)

In object-orientation, `abstract class` or `interface` is used to express abstract things that are difficult to change like this. In order to satisfy DIP, it must be designed to have a dependency relationship with `interface` or `abstract class` rather than a concrete class.

###Dependency Injection (DI)

`Dependency Injection (DI)` is a technique that injects dependencies from outside the class into the instance variable of the target object. Using this, you can change the external dependent objects of the target object from outside without changing the target object.

```java
public class Person {
    private Beverage beverage

    public void Person() {
        this.beverage = new Water();
        // this.beverage = new Coffee();
        // this.beverage = new Coke();
    }

    public void drink(){
        beverage.drink();
    }
}
```

First, let's look at the case where dependency injection is not used. Depending on the drink one wants to drink, the person can select the drink produced by the constructor. However, in the case of the above design, the constructor code must be changed as the beverage being consumed changes.

```java
public class Person {
    private Beverage beverage

    public void setBeverage(Beverage beverage) {
        this.beverage = beverage;
    }

    public void drink(){
        beverage.drink();
    }
}
```

When using dependency injection, you can reduce the coupling between objects and make the code more flexible by creating the necessary objects in `injection from the outside` rather than directly creating them in `inside the class`. *Dependency injection is receiving and using necessary objects from outside.*

Interface Separation Principle (ISP)
---

>`Interface Segregation Principle (ISP)`: The principle that the client should not be affected by functions that it does not use.

![multifunction-machine-class-diagram](/images/oop/solid-principle/multifunction-machine-class-diagram.jpg#center)

Let’s consider the case of a multi-function printer. A multi-function printer is a device that can be used not only for printing but also for various other functions. Therefore, it must be able to handle work requests from multiple clients.

Clients using the print function should not be affected by the fax or copy functions. However, since the above structure implements all functions within one class, it is likely to affect unrelated functions as well. Therefore, it must be separated from `interface` to `client-specific`.

![apply-isp-to-multifunction-machine-class-diagram](/images/oop/solid-principle/apply-isp-to-multifunction-machine-class-diagram.jpg#center)

It was designed so that each object that uses a multifunction device is provided with `interface`, which contains only the methods of interest. By designing it this way, the interface performs a kind of `firewall role` so that the client is not affected by changes made to its `deprecated` method.

###SRPs and ISPs

If a class performs `multiple responsibilities` without performing a single responsibility, it will result in a bloated class with `vast methods`. You can `Satisfy Your ISP` by splitting these classes into multiple classes with a single responsibility according to `SRP` and providing their own interfaces.

So, is SRP a necessary condition for ISP satisfaction? You can't just say that. For example, let's say there is a class to provide bulletin board functionality. If this class implements the `CRUD` method, it can be considered as `Satisfying SRP` since it performs responsibilities related to the bulletin board.

However, depending on the client, use may be limited to only `some features` on the bulletin board. For example, only administrators have the authority to delete posts. If the interface containing all methods of this class is `client-independent`, then it is `against the ISP`.

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
- [Wikipedia, Mammals](https://ko.wikipedia.org/wiki/%ED%8F%AC%EC%9C%A0%EB%A5%98)
- [wlsdud2194, [DI] What is Dependency Injection?, velog](https://velog.io/@wlsdud2194/what-is-di)

