---
title: "Collaboration Diagram"
translationKey: posts/cs/uml/collaboration-diagram
date: 2021-01-16T18:48:45+09:00
series:
  - UML
categories:
  - UML
tags:
  - UML
  - OOP
  - Java
draft: false
summary: >
  A tool to express design patterns in UML2.0
---

- Tool to express `Design Pattern` in UML 2.0
- Write `interaction` of the roles that objects play in specific situations

> `Collaboration diagram` used in UML 1. X was changed to `Communication diagram` in 2.0. The diagram name is different depending on the version, so be careful not to get confused.

How to write
---

- Use dotted oval symbol
- Expresses the roles that require cooperation within the oval and the connections between them
- When you want to specify the class of the role, express it as `<role_name>:<class_name>`

collaboration
---

![collaboration-diagram](/images/uml/collaboration-diagram/collaboration-diagram.jpg#center)

- Collaboration showing collateral loan relationships
- The roles `borrower`, `borrower`, and `collateral` are required.
 - Connect these roles with connectors because cooperation between them is required.
- Collaboration is an abstraction of the interaction of roles.
 - Can be reused in many system developments if applied in special situations

Collaboration Occurrence
---

> Application to special situations for collaboration

`Collaboration Occurence` expresses the application of collaboration in a specific situation.

![collaboration-occurence](/images/uml/collaboration-diagram/collaboration-occurence.jpg#center)

- `Collateral loan` collaboration is applied when a bank borrows money using a house as collateral.
 - Borrower = Bank
 -Collateral = house
 - Borrower = person
- In this case, `bank home mortgage loan` is an example of `mortgage loan` and is a collaboration occurrence.

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
