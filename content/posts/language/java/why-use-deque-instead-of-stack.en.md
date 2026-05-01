---
title: "Why Use Deque Instead Of Stack"
translationKey: posts/language/java/why-use-deque-instead-of-stack
date: 2023-05-05T22:30:19+09:00
draft: false
series:
  - Java
categories:
  - Java
  - Data Structure
tags:
  - Java
  - Data Structure
  - Queue
  - Stack
  - Deque
---

## Use Deque instead of Stack?

When solving algorithm problems using Java, I used to use `Deque` instead of `Stack`.
While searching online, I often came across posts that suggested using `Deque` instead of `Stack` class.

However, I think I was using it vaguely without thinking deeply about why I should use `Deque`.
We took this opportunity to learn more about the differences between `Stack` and `Deque`.

> A more complete and consistent set of LIFO stack operations is provided by the Deque interface and its implementations, which should be used in preference to this class. For example:

```java
Deque<Integer> stack = new ArrayDeque<Integer>();
stack.push(1);
```

Java official documentation talks about the `Stack` class as follows:
It says to use the `Deque` interface and its implementations rather than the `Stack` class.

In this post, we will take a closer look at `Queue`, `Stack`, and `Deque`.
Let’s find out what advantages `Deque` has over `Stack`.

## Queue/Stack/Deque

###Queue

`Queue` is a data structure that has the characteristics of `FIFO(First In First Out)`.
Java provides the corresponding data type through the `java.util. Queue<E>` interface.

#### Provided method<table>
 <thead>
 <tr>
 <td></td>
 <th scope="col" style="font-weight:normal; font-style:italic">Throws exception</th>
 <th scope="col" style="font-weight:normal; font-style:italic">Returns special value</th>
 </tr>
 </thead>
 <tbody>
 <tr>
 <th scope="row">Insert</th>
 <td>add(e)</td>
 <td>offer(e)</td>
 </tr>
 <tr>
 <th scope="row">Delete</th>
 <td>remove()</td>
 <td>poll()</td>
 </tr>
 <tr>
 <th scope="row">View</th>
 <td>element()</td>
 <td>peek()</td>
 </tr>
 </tbody>
</table>

`Queue` provides two methods for data manipulation.
Depending on which method you use, it is determined whether `an error occurred` or `Special Value` is returned when an exception occurs.

- `offer` method
 - If insertion was successful, `true` is returned.
 - If insertion is not done properly, `false` is returned.
- `poll` and `peek` methods
 - If data does not exist, `null` is returned.

#### Usage example

```java
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);
```

`Queue` usually uses `LinkedList` as its implementation.

### Stack

`Stack` is a data structure that has the characteristics of `LIFO(Last In First Out)`.
In Java, the corresponding data type is provided through the `java.util. Stack<E>` class.
It is implemented by inheriting the `Vector` class.

#### Provided method

| | Stack method |
| ------- | ------------ |
| Insert | push(e) |
| Delete | pop() |
| View | peek() |

#### Usage example

```java
Stack<Integer> stack = new Stack<>();
```

`Stack` is itself an implementation, so use it like this:

###Deque`Deque` is short for `Double Ended Queue`, commonly called a two-way queue.
Java provides the corresponding data type through the `java.util. Deque<E>` interface.
It is implemented by inheriting the `Queue` interface.

#### Provided method

<table>
 <thead>
 <tr>
 <td rowspan="2"></td>
 <th scope="col" colspan="2"> First Element (Head)</th>
 <th scope="col" colspan="2"> Last Element (Tail)</th>
 </tr>
 <tr>
 <th scope="col" style="font-weight:normal; font-style:italic">Throws exception</th>
 <th scope="col" style="font-weight:normal; font-style:italic">Special value</th>
 <th scope="col" style="font-weight:normal; font-style:italic">Throws exception</th>
 <th scope="col" style="font-weight:normal; font-style:italic">Special value</th>
 </tr>
 </thead>
 <tbody>
 <tr>
 <th scope="row">Insert</th>
 <td>addFirst(e)</td>
 <td>offerFirst(e)</td>
 <td>addLast(e)</td>
 <td>offerLast(e)</td>
 </tr>
 <tr>
 <th scope="row">Delete</th>
 <td>removeFirst()</td>
 <td>pollFirst()</td>
 <td>removeLast()</td>
 <td>pollLast()</td>
 </tr>
 <tr>
 <th scope="row">View</th>
 <td>getFirst()</td>
 <td>peekFirst()</td>
 <td>getLast()</td>
 <td>peekLast()</td>
 </tr>
 </tbody>
</table>Provides the ability to insert and delete data in both directions. It is separated into a method that does `Return Special Value`, which is the same as `Queue`, and a method that generates an error.

#### Stack/Queue methods vs. Deque method

| Queue method | Corresponding Deque method |
| ------------ | --------------------- |
| add(e) | addLast(e) |
| offer(e) | offerLast(e) |
| remove() | removeFirst() |
| poll() | pollFirst() |
| element() | getFirst() |
| peek() | peekFirst() |

`Deque` inherits the `Queue` interface, so it provides all the methods of `Queue`. The table above compares the methods of `Queue` and the methods of `Deque`.

| Stack method | Corresponding Deque method |
| ------------ | --------------------- |
| push(e) | addFirst(e) |
| pop() | removeFirst() |
| peek() | peekFirst() |

`Deque` also provides all of the methods of `Stack`. The table above compares the methods of `Stack` and the methods of `Deque`.

#### What happens when you call a Stack/Queue method

But then I suddenly became curious. How does `Deque` support the methods of `Queue` and `Stack`? So, I looked at the actual Java code using IntelliJ.

![How to implement the Stack function](https://vanslog.s3.ap-northeast-2.amazonaws.com/image/+2023-05-10++3.34.15.png)The `ArrayDeque` class, which is an implementation of Deque, provides Stack methods as follows.
When you use a method in `Stack`, such as the `push`, `pop`, and `peek` methods, you delegate to calling the corresponding method in `Deque`.
This also applies to the method of `Queue`.

#### Usage example

```java
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(1);

Queue<Integer> queue = new ArrayDeque<>();
queue.offer(1);

Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
```

> In the case of `Queue`, it is the parent interface of `Deque`, so the data type can be declared as `Queue`.

In conclusion, `Deque` can be accessed and used as a two-way queue, but
You can also use it like `Stack` or `Queue`.
This means that there will be no problem even if code that previously used `Stack` and `Queue` is changed to `Deque`.

## The father and the son

As mentioned briefly earlier, the `Stack` class is implemented by inheriting the `Vector` class.
The `Vector` class is a class that implements `List` and has been provided since Java 1.0.
However, its use is currently not recommended due to performance issues.
As a result, the use of the `Stack` class is also not recommended.

### Why Vector is not recommended

| Compare | Vector | ArrayList |
| ----------- | ----------- | ----------- |
| Synchronization Processing | O | X |
| thread safety | O | X |
| Performance | Relatively slow | Relatively fast |
| capacity increase | 2x | 1.5 times |

`Vector` consists of synchronized methods, making it safe in a multi-threaded environment. However, even in a single-threaded environment, overhead for synchronization processing may occur, resulting in performance degradation.
Therefore, in a single-threaded environment, using `ArrayList` has a performance advantage. Since Java 1.5, `Collections.synchronizedList` is provided, allowing synchronization even when using the `ArrayList` class.
Therefore, it is recommended to use `ArrayList` regardless of the thread environment.
In fact, most projects using Java today don't use `Vector`, they use `ArrayList`.

### Why using Stack is not recommended

| Compare | Stack | ArrayDeque |
| ----------- | ----------- | ----------- |
| Synchronization Processing | O | X |
| thread safety | O | X |
| Performance | Relatively slow | Relatively fast |

As you can easily see from the table, the relationship between `Stack` and `ArrayDeque` is very similar to the relationship between `Vector` and `ArrayList`.
In other words, the reason why `Stack` should not be used can be seen as the same as the reason why `Vector` should not be used.

However, `Deque` does not have a separate method for synchronization processing.
For example, methods such as `Collections.synchronizedDeque` are not provided.
So how can we use it safely in a multi-threaded environment?

```java
class SyncStack<E> {
    private final Deque<E> stack = new ArrayDeque<>();

    public synchronized void push(E e) {
        stack.push(e);
    }
}
```

These issues can be resolved through external synchronization.
If synchronization is performed externally as shown in the example above, `Deque` can be used safely even in a multi-threaded environment.

## Conclusion

This was an opportunity to take a closer look at Java data structures.
I decided to become a developer who can properly understand and use it rather than blindly using it because I was told to use it.

### Summary- `Stack` is implemented by inheriting `Vector`.
- `Vector` is not recommended for use due to poor performance in a single-threaded environment.
- Therefore, it is best not to use `Stack`, which inherits `Vector`.
- `ArrayDeque` is an implementation of `Deque` and supports both methods of `Stack` and `Queue`.
- Performance improvement can be expected by using `ArrayDeque` instead of `Stack`.
- If you want to use thread-safe `ArrayDeque`, use external synchronization.

## References

- [Java official documentation - Stack](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Stack.html)
- [Java Official Documentation - Vector](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Vector.html)
- [Java official documentation - Deque](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Deque.html)
- [Java official documentation - Collections.synchronizedList](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collections.html#synchronizedList(java.util.List))
- [ArrayList vs Vector synchronization & performance difference comparison](https://inpa.tistory.com/entry/JCF-🧱-ArrayList-vs-Vector---#)
