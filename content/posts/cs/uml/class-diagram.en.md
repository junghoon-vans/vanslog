---
title: "Class Diagram"
translationKey: posts/cs/uml/class-diagram
date: 2021-01-08T19:20:43+09:00
series:
  - UML
categories:
  - UML
tags:
  - UML
  - OOP
  - Java
  - General
draft: false
---


What is a class?
---

A class is a set of objects that have the same properties and behaviors. For example, what students majoring in software have in common is that they hear `majoring in software` as a fact and `same majoring class` as a fact. In this case, `student majoring in software` can be said to be a class of actual students.

Another way to define a class is to see it as `a blueprint for creating an instance (object)`. The source code below defines a student class majoring in software. Let's take a look at the code from the perspective of a blueprint for how classes create objects.

```java
public class Student{
 private String name;
 private String major = "SW";

 public Student(String name){
 this.name = name;
 }

 public void study() {
 System.out.println(" .");
 }
}
```

```java
Student student1 = new Student("1");
Student student2 = new Student("2");

student1.study();
student2.study();
```

Through the above code, two student objects with the same specifications were created. Both objects have the same characteristics except for their names. The major attribute is the same as `SW`, and the same sentence is output when the `study()` method is executed. This is why classes are the blueprint for creating objects.

>The same object is always created in the same blueprint.

UML Modeling
---

![student-uml-class](/images/uml/class-diagram/student-uml-class.png#center)

The result of notating the `Student` class using `UML` is as above. From the top, `ClassName`, `Property`, and `Operation` are described for each section. If there are no properties or operations, they can be omitted.

###access controller

|access controller|mark|explanation|
|-------------|------|------|
| public | + |Accessible to objects of any class|
| private | - |Only objects created in this class can be accessed.|
| protected | # |Only objects of subclasses that are in the same package as this class or have an inheritance relationship can be accessed.|
| package | ~ |Only objects of classes in the same package can be accessed.|

When defining class properties and operations, symbols such as `-` or `+` are used, which are access modifiers that define `visualization`. You can think of `private` and `public` in Java as written as above.

###Properties and Operations

|division|notation|
|------|--------|
|attribute|[Access Controller]Name: Type[Multiplicity Information] [=Initial Value]|
|calculation|[Access Controller] Name (Argument: Type): Return Type|

The format for notating properties and operations is as above. Class diagrams are widely used from the concept analysis stage to implementation, but in `analysis stage`, the main purpose is to define properties or operations rather than express them in detail. Afterwards, specific type information or visualization information is described in `design phase`.

>It means that anything contained in `[]` can be omitted.

relationship
---

In object-oriented programming, you rarely use just one object. Usually, objects are divided by function, and their interaction makes one software run. The relationship between these classes is expressed in `UML` as follows.

###association

It means that the classes are conceptually connected and is indicated by a solid line.

![uml-association-notation](/images/uml/class-diagram/uml-association-notation.jpg#center)

####two-way association

A case of `recognizing each other`, such as the relationship between a professor and a student class, is called a two-way relationship and is indicated by a `no arrow` solid line.

![professor-student-relation](/images/uml/class-diagram/professor-student-relation.png#center)

If you want to indicate that they are consulting, you can specify it in the solid line `top` as shown above. `rule name` in the association relationship can also be determined by defining `both ends` in the solid line. This can be used as `property` to refer to each other in the later stages of program implementation.

####one-way association

One student can take multiple classes. If this is modeled through UML, it can be expressed as follows.

![student-course-relation](/images/uml/class-diagram/student-course-relation.png#center)

At this time, the arrow is pointing from the student to the class, which means it is `student recognizes the class`, and conversely, the class does not recognize the student. This case is called `one-way association`.

In the diagram above, there is a notation called `1..*`, which represents `multiplicity`. Multiplicity means `number of related objects`.

>`*` means `0 or more`, and `..` represents the range. Therefore, `1..*` means `1 or more`. If there is only one object, it may be omitted.

####Many-to-many associations

If you think about the previous example carefully, you may realize that something is strange. In the real world, there is never a case where only one student takes a class. Usually, multiple students take multiple classes. If you express this, the picture below will appear.

![many-to-many-relation](/images/uml/class-diagram/many-to-many-relation.png#center)

The relationship that `many objects - many objects` has is called `many-to-many association relationship`, and this is generally expressed as `two-way association relationship` in UML.

![association-class](/images/uml/class-diagram/association-class.jpg#center)

However, if a student wants to save grade information generated while taking a class, where should it be stored? If you store grade information as is in a student or class, it will be expressed as follows.

‘Student Hong Gil-dong is an A+’ or ‘He received an A+ in the object-oriented modeling class’

However, it is missing information about who earned the grade in which class. Therefore, it is correct to create `separate class` and save student grades rather than saving them to the student or class. The same class as `Transcript` used at this time is called `Associated class`.

![association-class-to-general-class](/images/uml/class-diagram/association-class-to-general-class.jpg#center)

The actual implementation of `associative class` is achieved by converting `generic class` to `one-way association`.

>When actually implementing a program, `two-way association` is not used!

####recursive association

![superior-subordinate-relation](/images/uml/class-diagram/superior-subordinate-relation.jpg#center)

Associations are sometimes recursive. For example, in the military, there is a relationship called `senior` and `successor`. A soldier who is a senior to me is a junior to someone, and a soldier who is a junior to me is also a senior to someone.

![recursive-association](/images/uml/class-diagram/recursive-association.jpg#center)

In this case, a contradiction arises where the class called soldier belongs to two classes, namely senior and junior, at the same time. However, creating the two classes separately lacks flexibility. In these cases, `recursive association` is used.

However, the problem of `relationship loop` remains in recursive associations. For example, `rock, paper, scissors` is a game where scissors beats paper, paper beats rock, and rock beats scissors. If a loop exists like this, it must be excluded by setting a constraint to `{hierarchy}`.

>`{Hierarchy}` means that `hierarchy` exists between objects and `cycle` does not exist.

###generalization relationship

It is a relationship between two classes when one class contains another class `super concept`.

![uml-generalization-notation](/images/uml/class-diagram/uml-generalization-notation.jpg#center)

`Child class (subclass)` can inherit properties or operations from `parent class (superclass)`. That is why the generalization relationship is also called `inheritance relationship`.

![home-appliance-inheritance](/images/uml/class-diagram/home-appliance-inheritance.jpg#center)

Usually, a generalization relationship is called a `is-a-kind-of` relationship. The relationship between home appliances and washing machines can be described as `washing machine is-a-kind-of home appliance`.

###set relationship

A set relationship is a special case of an association relationship and is used when you want to clearly specify the relationship between the whole and the parts. There are two types of set relationships: `aggregation` and `composition`.

####intensive relationship

Indicates that one object contains another object.

![uml-aggregation-notation](/images/uml/class-diagram/uml-aggregation-notation.jpg#center)

- Empty diamond in class direction pointing to whole
- Can `share with other objects` partial objects
- `Lifetime` of whole object and partial object are independent

####composite relationship

It is a relationship in which a partial object belongs to the whole object.

![uml-composition-notation](/images/uml/class-diagram/uml-composition-notation.jpg#center)

- A filled diamond in the class direction pointing to the whole.
- Cannot `share with other objects` partial object
- `Lifetime` of a partial object depends on the whole object

####difference

Aggregation relationships and composition relationships may seem similar at first glance, but they have a big difference. The most important difference is `Lifetime`. When the entire object is destroyed `if the partial object remains` it is `aggregate relation`. In the opposite case, you can think of it as `composite relationship`.

Let’s find out the difference between the two using the example of assembling a computer.

![computer-components-aggregation](/images/uml/class-diagram/computer-components-aggregation.jpg#center)

```java
public class Computer {
 private MainBoard mainBoard;
 private CPU cpu;
 private Memory memory;
 private PowerSupply powerSupply;

 public Computer(MainBoard mainBoard, CPU cpu, Memory memory, PowerSupply powerSupply){
 this.mainBoard = mainBoard;
 this.cpu = cpu;
 this.memory = memory;
 this.powerSupply = powerSupply;
 }
}
```

Computer objects are created by receiving an externally created motherboard, CPU, memory, and power supply. Therefore, even if a computer object is destroyed, the partial objects that make up the computer do not disappear but remain in memory. Therefore, we can see that the above source code represents `aggregate relationship`.

![computer-components-composition](/images/uml/class-diagram/computer-components-composition.jpg#center)

```java
public class Computer {
 private MainBoard mainBoard;
 private CPU cpu;
 private Memory memory;
 private PowerSupply powerSupply;

 public Computer(){
 this.mainBoard = new MainBoard();
 this.cpu = new CPU();
 this.memory = new Memory();
 this.powerSupply = new PowerSupply();
 }
}
```

Unlike the previous example, the code above creates the computer and its components are created at the same time. Therefore, the life cycle of the elements depends on the entire object. It can be viewed as `composite relationship` because partial objects disappear at the same time as the computer is destroyed.

###dependency

This is a relationship that appears when using functions provided by other classes.

![uml-dependency-notation](/images/uml/class-diagram/uml-dependency-notation.jpg#center)

In general, there are three cases where one class uses another class:

- Reference from property of class
- Used as an argument for operations
- Reference to local object inside method

![dependency-relation](/images/uml/class-diagram/dependency-relation.jpg#center)

The diagram above shows that a person owns a car and recharges the car at a gas station. At this time, the relationship between `person-car` is `associative relationship`, but the relationship between `car-gas station` is `dependency relationship`.

```java
public class Person {
 private Car car;

 public void setCar(Car car){
 this.car = car;
 }
 ...
}

public class Car {
 public void fillGas(GasPump p){
 p.getGas(amount);
 ...
 }
}
```

Since the car people ride does not change every time, the `Car` object is referred to as an attribute of the `Person` class. On the other hand, the main element of refueling the car is not the same every time, so it is implemented through `argument` or `local object`.

###materialization relationship

It is the relationship between an interface and the class that instantiates its `responsibilities`s.

>Responsibility refers to `what an object must do` to `what it can do`.

![uml-realization-notation](/images/uml/class-diagram/uml-realization-notation.jpg#center)

The interface itself is not an object that actually carries out responsibilities. An object created through instantiation performs the responsibilities defined in the interface.

![flyable-interface](/images/uml/class-diagram/flyable-interface.jpg#center)

For example, classes `Plane` and `Bird` that instantiate an interface called `Flyable`, which contains the responsibility for flying, must implement the responsibilities of that interface. In this respect, an interface can be viewed as representing things that have certain common capabilities. That is why the materialization relationship is called the `can-do-this` relationship.

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
