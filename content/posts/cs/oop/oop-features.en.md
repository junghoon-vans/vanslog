---
title: "Oop Features"
date: 2021-01-09T17:18:32+09:00
series:
  - OOP
categories:
  - OOP
tags:
  - OOP
  - Java
draft: false
summary: >
  Four characteristics of object-oriented programming (OOP)
---


Abstraction
---

It is a means of understanding the `common characteristics` of concrete objects and treating them as a concept. In other words, it is about extracting key concepts or functions from complex data, modules, systems, etc.

These abstractions are essential for creating classes in `Object-Oriented Programming (OOP)`. If there is no abstraction, every object must be defined and implemented in detail.

Encapsulation
---

`Encapsulation` is an object-oriented design principle that allows `high cohesion` and `low cohesion` through information hiding.

###Cohesion and binding

The design of a class should be reusable, extensible, and maintainable. For this to happen, `cohesion` must be low and `cohesion` must be high.

- Cohesion
 -The degree to which elements within a class or module are closely related
- Coupling
 -Degree to which a function depends on other classes or modules to perform it

###information hiding

Information that does not need to be known is restricted from being accessed from the outside. We can drive a car without knowing how it works, and we can use a computer well without knowing how it works.

```java
public class ArrayStack {
 public int top;
 public int[] itemArray;
 public int stackSize;

 public ArrayStack(int stackSize) {
 itemArray = new int[stackSize];
 top = -1;
 this.stackSize = stackSize;
 }

 public void push(int item) { // item
 ...
 }

 public int pop() { // item
 ...
 }

 public int peak() { // item
 ...
 }
 ...
}
```

`ArrayStack` is a stack implemented using an array. You can add items using the `push` function and return them through `pop`.

```java
public class StackClient {
 public static void main(String[] args){
 ArrayStack st = new ArrayStack(10);
 st.itemArray[++st.top] = 20;
 System.out.print(st.itemArray[st.top]);
 }
}
```

However, ArrayStack's properties are all designated as `public`, so they can be accessed from the outside. In this case, you can directly handle the array without using the `push` function, and a strong coupling occurs between `StackClient` and `ArrayStack`.

If the implementation of the stack changes, the source code of `StackClient` may also need to be changed. Therefore, you should avoid directly handling stack properties. Therefore, the properties of `ArrayStack` are prevented from being accessed by external classes by setting the access modifier to `private`. Now that you can no longer manipulate the array directly, `StackClient` must be modified as follows.

```java
public class StackClient {
 public static void main(String[] args){
 ArrayStack st = new ArrayStack(10);
 st.push(20);
 System.out.print(st.peek());
 }
}
```

Generalization
---

It is a process of highlighting the common characteristics of various entities and establishing them as a single concept or law.

- The generalization relationship is called an inheritance relationship from an object-oriented programming perspective.
- Therefore, it is often used to emphasize only the reuse of properties or functions.
- This is an extremely limited view of the generalization relationship.

###Generalization for encapsulation

Generalization can also be seen as the concept of `encapsulation (or hiding)` a child class to the outside world.

![generalization-is-encapsulation](/images/oop/oop-features/generalization-is-encapsulation.jpg#center)

For example, when we classify cars, we classify them into various manufacturers such as BMW, Hyundai, and Benz. Assuming a situation where a designated driver is driving, the vehicle type does not have a significant effect on driving. From the perspective of the `person` class, you only need to be interested in the `car` class, not the specific car. Therefore, the specific car class can be seen as `concealed`.

###Generalization from a set theory perspective

The generalization relationship can also be interpreted from a ‘set theory’ perspective.

![generalization-as-set-theory](/images/oop/oop-features/generalization-as-set-theory.jpg#center)
The parent class A corresponds to the entire set A, and its subsets A1, A2, and A3 respectively correspond to child classes of A. At this time, the following relationship must be established:

- `A = A1 ∪ A2 ∪ A3`
- `A1 ∩ A2 ∩ A3 = ø`

And the following constraints also exist.

![constraints-in-generalization-relationships](/images/oop/oop-features/constraints-in-generalization-relationships.jpg#center)

- `{disjoint}`: A child class object cannot belong to two classes at the same time.
- `{complate}`: There is only one `object of the child class` corresponding to the object of the child class and `object of the parent class` corresponding to the object of the parent class.

By creating a generalization relationship from a set theory perspective, the association relationship can be simplified.

![association-in-web-shop-member](/images/oop/oop-features/association-in-web-shop-member.jpg#center)

If an online shopping mall is classifying `VIP customer` and `regular customer`, you can draw a diagram like the one above. Each member can have an association with each item, but purchasing the item has nothing to do with the level.

![use-set-theory-to-simplify-association](/images/oop/oop-features/use-set-theory-to-simplify-association.jpg#center)

In other words, the association relationship with the object class is `common association relationship` of the child class of the object class. Therefore, we can `simplify` the diagram by associating object classes with member classes.

>Generalization from a set theoretic perspective can be viewed as a process of dividing something into mutually exclusive subsets.

What should we do if criteria for classifying members are added? Web shopping malls sometimes provide services not only to domestic members but also to `foreign members`. When providing different services depending on the member's country, this distinction will be necessary.

In UML, this distinction is called `discriminator` and discriminator information is displayed next to the line indicating the generalization relationship. However, in this case, members are classified according to nine criteria (payment amount, region), and one instance that can belong to multiple classes at the same time is called `multiple classification`. Expressed using the `<<multiple>>` stereotype.

![discriminator-and-multiple-classification](/images/oop/oop-features/discriminator-and-multiple-classification.jpg#center)

In general, if the generalization relationship for each discriminator is completely independent, there is no problem. However, due to changes/additions in requirements, both `situations where the generalization relationship is not independent` must also be considered.

For example, in the diagram above, it is easy to implement providing a discount coupon to `VIP members`, but providing a gift to `regular level foreign members` is impossible.

![classes-for-all-combinations](/images/oop/oop-features/classes-for-all-combinations.jpg#center)

One way to handle this is to create `classes corresponding to all classifiable combinations`.

Polymorphism
---

This is the ability for objects of different classes to act in their own way when they receive the same message.

Let’s consider the car example we saw earlier in [Generalization for encapsulation](#Generalization for encapsulation). It is possible to drive a car regardless of its brand. So, from the perspective of the `people` class, you only need to be interested in the `car` class. The reason this is possible is because there is `polymorphism`.

```java
abstract class Car {
 public abstract void ride();
}

public class BMW extends Car {
 public void ride() { ... }
}

public class HYUNDAI extends Car {
 public void ride() { ... }
}

public class BENZ extends Car {
 public void ride() { ... }
}

public class Main {
 public static void rideCars(Car[] cars) {
 for(Car car: cars){
 car.ride();
 }
 }

 public static void main(String[] args){
 Car[] cars = {new BMW(), new HYUNDAI(), new BENZ()};
 rideCars(cars);
 }
}
```

When you use polymorphism, you can program independently of which class object is currently being referenced. Therefore, even if a new car class is added as a child class, the code will not be affected.

The reason this is possible is because the reference variable of the parent class can be `reference an object of the child class` when in a generalization relationship. However, only the parent class `inherited variables and methods` can be accessed by the reference variable of the parent class.

Peter Codd's Inheritance Rules
---

`Peter Code` created rules that strictly limit the use of inheritance to prevent its misuse.

- There must not be a `role performance` relationship between the child class and the parent class.
- An instance of one class should never need to be converted to an object of another subclass.
- The child class must only perform `extends` without overriding or redefining the parent class's responsibilities.
- Child classes should not inherit `classes that perform a utility role` just to reuse some functionality.
- The child class must specialize `role`, `transaction`, `device`, etc.

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
