---
title: "[Java] 왜 Stack 대신 Deque를 사용하는가?"
date: 2023-05-05T22:30:19+09:00
draft: false
series:
  - ☕️ Java 스터디
categories:
  - ☕️ Java
  - 자료구조
tags:
  - ☕️ Java
  - 자료구조
  - Queue
  - Stack
  - Deque
---

## Stack 대신 Deque를 사용하라?

평소에 자바를 사용해 알고리즘 문제를 풀면서 `Stack` 대신 `Deque`를 사용하곤 했습니다.
구글링을 하다 우연히 `Stack` 클래스 대신 `Deque`를 사용하라는 글을 자주 마주한 것이 계기였습니다.

하지만 `Deque`를 사용해야 되는 이유에 대해 깊은 생각없이 막연히 사용하고 있던 것 같아,
이번 기회에 `Stack`과 `Deque`의 차이점을 자세히 알아보았습니다.

> A more complete and consistent set of LIFO stack operations is provided by the Deque interface and its implementations, which should be used in preference to this class. For example:

```java
Deque<Integer> stack = new ArrayDeque<Integer>();
stack.push(1);
```

자바 공식 문서에서는 `Stack` 클래스에 대해 다음과 같이 이야기합니다. 
`Stack` 클래스를 사용하기 보다는 `Deque` 인터페이스와 그 구현체를 사용하라고 말합니다.

이번 포스팅에서는 `Queue`와 `Stack` 그리고 `Deque`에 자세히 살펴보고,
`Deque`가 `Stack` 대비 가지는 장점이 무엇인지에 대해 알아보겠습니다.

## Queue/Stack/Deque

### Queue

`Queue`는 `FIFO(First In First Out)`의 특징을 가지고 있는 자료구조입니다.
자바에서는 `java.util.Queue<E>` 인터페이스를 통해 해당 자료형을 제공합니다.

#### 제공 메서드

<table>
 <thead>
 <tr>
   <td></td>
   <th scope="col" style="font-weight:normal; font-style:italic">Throws exception</th>
   <th scope="col" style="font-weight:normal; font-style:italic">Returns special value</th>
 </tr>
 </thead>
 <tbody>
 <tr>
   <th scope="row">삽입</th>
   <td>add(e)</td>
   <td>offer(e)</td>
 </tr>
 <tr>
   <th scope="row">삭제</th>
   <td>remove()</td>
   <td>poll()</td>
 </tr>
 <tr>
   <th scope="row">조회</th>
   <td>element()</td>
   <td>peek()</td>
 </tr>
 </tbody>
</table>

`Queue`는 데이터 조작에 필요한 메서드를 두 개씩 제공합니다.
어떤 메서드를 사용하느냐에 따라 예외 상황이 발생했을 때, `에러를 발생`시킬 지 `Special Value`를 리턴할 지가 결정됩니다.

- `offer` 메서드
  - 삽입이 제대로 이루어진 경우 `true` 반환
  - 삽입이 제대로 이루어지지 않은 경우 `false` 반환
- `poll`과 `peek` 메서드
  - 데이터가 존재하지 않는 경우 `null` 값을 반환

#### 사용 예제

```java
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);
```

`Queue`는 보통 구현체로 `LinkedList`를 사용합니다.

### Stack

`Stack`은 `LIFO(Last In First Out)`의 특징을 가지고 있는 자료구조입니다.
자바에서는 `java.util.Stack<E>` 클래스를 통해 해당 자료형을 제공합니다.
이는 `Vector` 클래스를 상속받아 구현되어 있습니다.

#### 제공 메서드

|         | Stack 메서드 |
| ------- | ------------ |
| 삽입    | push(e)      |
| 삭제    | pop()        |
| 조회    | peek()       |

#### 사용 예제

```java
Stack<Integer> stack = new Stack<>();
```

`Stack`은 그 자체로 구현체이므로 다음과 같이 사용합니다.

### Deque

`Deque`는 `Double Ended Queue`의 약자로, 흔히 양방향 큐라고 불리는 것입니다.
자바에서는 `java.util.Deque<E>` 인터페이스를 통해 해당 자료형을 제공합니다.
이는 `Queue` 인터페이스를 상속받아 구현되어 있습니다.

#### 제공 메서드

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
   <th scope="row">삽입</th>
   <td>addFirst(e)</td>
   <td>offerFirst(e)</td>
   <td>addLast(e)</td>
   <td>offerLast(e)</td>
 </tr>
 <tr>
   <th scope="row">삭제</th>
   <td>removeFirst()</td>
   <td>pollFirst()</td>
   <td>removeLast()</td>
   <td>pollLast()</td>
 </tr>
 <tr>
   <th scope="row">조회</th>
   <td>getFirst()</td>
   <td>peekFirst()</td>
   <td>getLast()</td>
   <td>peekLast()</td>
 </tr>
 </tbody>
</table>

양방향으로 데이터를 삽입하고 삭제하는 기능을 제공합니다. `Queue`와 동일하게 `Special Value를 리턴`하는 메서드와 에러를 발생하는 메서드로 분리되어 있습니다.

#### Stack/Queue 메서드 vs. Deque 메서드

| Queue 메서드 | 대응되는 Deque 메서드 |
| ------------ | --------------------- |
| add(e)       | addLast(e)            |
| offer(e)     | offerLast(e)          |
| remove()     | removeFirst()         |
| poll()       | pollFirst()           |
| element()    | getFirst()            |
| peek()       | peekFirst()           |

`Deque`는 `Queue` 인터페이스를 상속한 것이므로 `Queue`의 메서드를 모두 제공합니다. 위 표는 `Queue`의 메서드와 `Deque`의 메서드를 비교한 것입니다.

| Stack 메서드 | 대응되는 Deque 메서드 |
| ------------ | --------------------- |
| push(e)      | addFirst(e)           |
| pop()        | removeFirst()         |
| peek()       | peekFirst()           |

`Deque`는 `Stack`의 메서드 또한 모두 제공합니다. 위 표는 `Stack`의 메서드와 `Deque`의 메서드를 비교한 것입니다.

#### Stack/Queue의 메서드를 호출하면 벌어지는 일

그런데 문득 궁금해졌습니다. `Deque`는 `Queue`나 `Stack`의 메서드 어떻게 함께 지원하고 있는 것인지가 말이죠. 그래서 인텔리제이를 사용해서 실제 자바 코드를 살펴보았습니다.

![Stack 함수를 구현하는 방법](https://vanslog.s3.ap-northeast-2.amazonaws.com/image/스크린샷+2023-05-10+오전+3.34.15.png)

Deque의 구현체인 `ArrayDeque` 클래스는 Stack의 메서드를 다음과 같이 제공하고 있습니다.
`push`, `pop`, `peek` 메서드와 같이 `Stack`의 메서드를 사용하면 이것에 대응되는 `Deque`의 메서드를 호출하도록 위임하는 것입니다.
이는 `Queue`의 메서드에 대해서도 마찬가지로 적용되어 있습니다.

#### 사용 예제

```java
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(1);

Queue<Integer> queue = new ArrayDeque<>();
queue.offer(1);

Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
```

> `Queue`의 경우 `Deque`의 부모 인터페이스이므로, 데이터 타입을 `Queue`로 선언할 수 있습니다.

결론적으로 `Deque`는 양방향 큐로써 접근하고 사용할 수도 있지만,
`Stack`이나 `Queue`처럼 사용할 수도 있습니다.
이는 곧 기존에 `Stack`과 `Queue`를 사용하던 코드를 `Deque`로 변경하더라도 문제가 없다는 것을 의미합니다.

## 그 아버지에 그 아들

`Stack` 클래스는 앞서 잠시 언급했듯이 `Vector` 클래스를 상속하여 구현되어 있습니다.
`Vector` 클래스는 `List`를 구현한 클래스로 자바 1.0부터 제공되어 왔습니다.
하지만 현재는 성능 상의 문제로 사용이 권장되지 않고 있습니다.
결과적으로 `Stack` 클래스도 사용이 권장되지 않게 된 것이죠.

### Vector의 사용이 권장되지 않는 이유

| 비교        | Vector      | ArrayList   |
| ----------- | ----------- | ----------- |
| 동기화 처리 | O           | X           |
| 쓰레드 안전 | O           | X           |
| 성능        | 비교적 느림 | 비교적 빠름 |
| 용량 증가   | 2배         | 1.5배       |

`Vector`는 동기화된 메서드로 구성되어 있어 멀티 쓰레드 환경에서 안전합니다. 하지만 단일 쓰레드 환경에서도 동기화 처리에 대한 오버헤드가 발생하여 성능 저하가 발생할 수 있습니다.
따라서 단일 쓰레드 환경에서는 `ArrayList`를 사용하는 것이 성능상 유리합니다.

자바 1.5 이후부터는 `Collections.synchronizedList`가 제공되어  `ArrayList` 클래스를 사용하더라도 동기화 처리를 할 수 있습니다.
따라서 쓰레드 환경에 무관하게 `ArrayList`를 사용하는 것이 좋습니다.
실제로 오늘날 자바를 사용하는 대부분의 프로젝트에서는 `Vector`를 사용하지 않고 `ArrayList`를 사용하고 있습니다.

### Stack의 사용이 권장되지 않는 이유

| 비교        | Stack       | ArrayDeque  |
| ----------- | ----------- | ----------- |
| 동기화 처리 | O           | X           |
| 쓰레드 안전 | O           | X           |
| 성능        | 비교적 느림 | 비교적 빠름 |

표를 보시면 쉽게 눈치채시겠지만, `Stack`과 `ArrayDeque`의 관계는 `Vector`와 `ArrayList`의 관계와 매우 유사합니다.
다시 말해 `Stack`을 사용하면 안되는 이유는 `Vector`를 사용하면 안되는 이유와 동일하다고 볼 수 있는 것이죠.

하지만 `Deque`는 별도의 동기화 처리를 위한 메서드가 없습니다.
이를테면 `Collections.synchronizedDeque`와 같은 메서드가 제공되지 않는다는 것이죠.
그렇다면 어떻게 멀티 쓰레드 환경에서 안전하게 사용할 수 있을까요?

```java
class SyncStack<E> {
    private final Deque<E> stack = new ArrayDeque<>();

    public synchronized void push(E e) {
        stack.push(e);
    }
}
```

이러한 문제는 외부 동기화를 통해 해결할 수 있습니다.
위 예시와 같이 동기화 처리를 외부에서 해주면 `Deque`를 멀티 쓰레드 환경에서도 안전하게 사용할 수 있습니다.

## 마치며

이번 기회에 자바 자료구조에 대해 자세히 살펴보는 시간을 가질 수 있었습니다.
무작정 사용하라고 해서 사용하기보다는 제대로 이해하고 이용할 수 있는 개발자가 되어야겠다는 다짐을 하였습니다.

### 요약

- `Stack`은 `Vector`를 상속받아 구현되어 있다.
- `Vector`는 단일 쓰레드 환경에서의 성능 저하로 사용이 권장되지 않는다.
- 따라서 `Vector`를 상속한 `Stack` 또한 사용하지 않는 것이 좋다.
- `ArrayDeque`는 `Deque`의 구현체이며 `Stack`과 `Queue`의 메서드를 모두 지원한다.
- `Stack` 대신 `ArrayDeque`를 사용하면 성능 향상을 기대할 수 있다.
- 쓰레드 안전한 `ArrayDeque`를 사용하기를 원한다면 외부 동기화를 사용하자.

## 참고문헌

- [Java 공식 문서 - Stack](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Stack.html)
- [Java 공식 문서 - Vector](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Vector.html)
- [Java 공식 문서 - Deque](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Deque.html)
- [Java 공식 문서 - Collections.synchronizedList](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collections.html#synchronizedList(java.util.List))
- [ArrayList vs Vector 동기화 & 성능 차이 비교](https://inpa.tistory.com/entry/JCF-🧱-ArrayList-vs-Vector-동기화-차이-이해하기#)
