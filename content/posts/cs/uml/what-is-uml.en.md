---
title: "What Is Uml"
translationKey: posts/cs/uml/what-is-uml
date: 2021-01-03T11:22:49+09:00
series:
  - UML
categories:
  - UML
tags:
  - UML
  - OOP
  - General
draft: false
summary: >
  UML, a language for modeling object-oriented applications
---

concept
---

`United Modeling Language` is standardized to facilitate smooth communication between developers in `system development process`, such as requirements analysis, system design, and system implementation.

UML is not the first to describe programs with diagrams. In the past, `OMT` was used for object-oriented modeling, which became the prototype of UML.

diagram
---

### Structure diagram

| Diagram Type | Purpose |
|----------------|------|
| Class | Expression of relationships between classes that make up the system |
| Object | Show object information |
| Composite Structure | Representation of internal structure of classes and components of complex structure |
| Deployment | Expression of physical structure of execution system including S/W, H/W, N/W |
| Component | Expression of relationships between component structures |
| Package | Group several model elements, including classes and use cases, to form a package and express relationships between packages |

### Behavior diagram

| Diagram Type | Purpose |
|----------------|------|
| Activity | Expression of the process in which a business process or calculation is performed |
| State Machine | Representation of the life cycle of an object |
| Use Case | Expressing system behavior from the user's perspective |
| Interaction | Classified into 4 categories according to purpose |

### Interaction diagram

| Diagram Type | Purpose |
|----------------|------|
| Sequence | Expression of interaction between objects over time |
| Interaction overview | Representing control flow between multiple interaction diagrams |
| Communication | Interaction expression centered on relationships between objects |
| Timing | Explicitly express object state changes and time constraints |`UML 2.0` provides 13 diagrams that express system structure and operation. The reason why so many types exist is to model the system with `diverse perspectives`.

Features
---

- Visualization language
 - Visually displays modeling results
- Specification language
 - Modeling accurately and completely
- Building language
 - Allows you to build a system
- Documentation language
 - Role of control, evaluation, and communication of the system

tools
---

- [draw.io](http://www.draw.io)
 - Free web-based UML drawing application
- starUML
 - Free desktop-based UML drawing application

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
