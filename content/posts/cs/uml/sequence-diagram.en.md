---
title: "Sequence Diagram"
date: 2021-01-23T16:10:03+09:00
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
  Diagram representing the interaction of objects
---

It is one of the diagrams that represents the interaction of objects and shows the transmission of messages between objects and their order.

component
---

### object

- Expressed at the top
- List objects from left to right
- Expressed in `object name: class name` format

### Lifeline

- Dotted line running underneath the object
- means that the object exists

### Active section

- Narrow and long rectangle along the life line
- A state in which an object is executing an operation
- Set appropriately considering execution time

message
---

- Format
- `[sequence number][guard]: return value:=message name([argument list])`
- Indicated by arrow
 - Beginning: Sending object
 - End part: receiving object
- Guard
- `Conditions that must be satisfied` until the message is sent

### type

![message-types](/images/uml/sequence-diagram/message-types.jpg#center)

| Type | meaning |
|:----:|:----:|
| motivational message | You cannot perform any of the following actions until the message has finished executing |
| asynchronous message | You can perform the following actions without waiting for the message to finish executing |
| return message | Means the message has ended and is not required to be |
| own message | A message to yourself |

### Stereotypes

- <<create>>
 - Message that creates an object
- <<destroy>>
 - Message that destroys an object
 - Put `X` at the end of the life line.

frame
---

- Provides a place for labels including boundaries, types, and names in every diagram (UML 2.0)
- Displayed as a box surrounding the diagram
- Display the diagram type and name in the left corner of the box
 - sd: sequential diagram
 - uc: Use case diagram
 - act: activity diagram

![sequence-diagram-using-frame](/images/uml/sequence-diagram/sequence-diagram-using-frame.jpg#center)

The process of lending books to members in a library is displayed in a sequential diagram frame. However, the above diagram only shows cases where rental is successful. What should I do if I want to indicate a case of failure?

![sequence-diagram-using-alt-keyword](/images/uml/sequence-diagram/sequence-diagram-using-alt-keyword.jpg#center)

In this case, you can use `alt keyword` to allow interaction to be performed selectively based on conditions.

![sequence-diagram-using-loop-keyword](/images/uml/sequence-diagram/sequence-diagram-using-loop-keyword.jpg#center)If you want to express repeating the process of verifying a member's password several times, you can use `loop keyword`.

> When simply referencing another sequential diagram, use `ref keyword`.

References
---

- [Inseong Jeong, Heungseok Chae, 『JAVA Object-Oriented Design Pattern』, Hanbit Media](http://www.yes24.com/Product/Goods/12501269)
