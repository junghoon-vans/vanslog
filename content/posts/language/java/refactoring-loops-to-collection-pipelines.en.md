---
title: "Refactoring Loops To Collection Pipelines"
date: 2021-07-19T13:03:57Z
series:
  - Java
categories:
  - Java
tags:
  - Java
  - General
  - Coding Test
draft: false
summary: >
  When is it best to use streams introduced in Java 8?
---

While preparing for the coding test, I am re-solving the problems provided by `Programmers`. While solving the problem [the athlete who did not finish the race](https://programmers.co.kr/learn/courses/30/lessons/42576), I suddenly remembered that there was a saying in the book `refactoring` called [**Change loops to pipelines**](/posts/refactoring/smell-in-code/#repetition). I decided to change all the code previously written in loops to pipelines.

> Pipeline is one of the techniques of functional programming and refers to a structure that processes data in multiple stages.

Refactoring
---

### Code written with loops

```java
class Solution {
 public String solution(String[] participant, String[] completion) {
 String answer = "";
 HashMap<String, Integer> pm = new HashMap<>();
 for (String element: participant){
 pm.put(element, pm.getOrDefault(element, 0)+1);
 }
 for (String element: completion){
 pm.put(element, pm.get(element)-1);
 }
 for (String key: pm.keySet()){
 if(pm.get(key) != 0){
 answer += key;
 }
 }
 return answer;
 }
}
```

The code that solved the same problem a long time ago is as follows. `for statement` is a repeated code like `three times`, but because the logic code is surrounded by code blocks, it is not readable at a glance.

### Pipelined code

```java
class Solution {
 public String solution(String[] participant, String[] completion) {
 Map<String, Integer> map = new HashMap<>();
 Arrays.stream(participant)
 .forEach(s -> map.put(s, map.getOrDefault(s, 0)+1));
 Arrays.stream(completion)
 .forEach(s -> map.put(s, map.get(s)-1));
 Optional<String> answer = map.keySet().stream()
 .filter(k -> map.get(k)!=0)
 .findFirst();
 return answer.get();
 }
}
```

When solving the problem this time, I followed the advice and applied the pipeline. I changed all loops to `Stream`, which actually made the code concise and easier to read.

Through solving this problem, I was able to see that the advantage of the pipeline is that it **clears the intent of the code** by focusing on whether `what` work is done, rather than whether data is manipulated `how`.


Isn't that always a good thing?
---

In `refactoring` and `clean code`, performance aspects are important, but **ultimately, code that is easier for people to read is better code**. If you think about it that way, doesn't pipelined code have only advantages? The same task can be written with easy-to-read code. However, contrary to what I thought, there was negative content on the Internet.

One article said that you should refrain from using `repeat` and `Stream` because there is a dramatic difference in performance. In fact, I was curious about the speed difference between code written with loops and code using a pipeline, so I compared the speed difference between the two codes.| | loop | pipeline |
|--------|--------|-----------|
| Test 1 | 1.58ms | 1.42ms |
| Test 2 | 2.17ms | 2.16ms |
| Test 3 | 2.59ms | 2.29ms |
| Test 4 | 7.06ms | 3.18ms |
| Test 5 | 2.89ms | 3.13ms |
| Test 6 | 55.80ms | 41.25ms |
| Test 7 | 83.86ms | 95.49ms |
| Test 8 | 82.06ms | 109.19ms |
| Test 9 | 106.20ms | 134.83ms |
| Test 10 | 119.71ms | 81.35ms |

As a result, `similar execution time` came out no matter which one was used. No, sometimes better performance was achieved when using the pipeline.

conclusion
---

Considering the results of executing the preceding code, there is no need to worry too much. When Java 8 was released, functional programming techniques were being applied, so there may have been issues with optimization. But now that Java 16 has been released, it has probably gone through a lot of optimization.

Above all, giving up readability for performance alone can cause `greater cost` in terms of maintenance. In the general case, the function is developed through a pipeline, but in cases where `critical performance degradation` is present, this problem can be solved by using a loop statement.
