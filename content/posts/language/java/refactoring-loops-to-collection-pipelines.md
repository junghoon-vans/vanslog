---
title: "[Java] 반복문을 파이프라인으로 바꿔라"
date: 2021-07-19T13:03:57Z
series:
  - ☕️ Java 스터디
categories:
  - ☕️ Java
tags:
  - ☕️ Java
  - 리팩터링
  - 스트림
  - 파이프라인
  - 코딩테스트
draft: false
summary: >
  자바8에서 등장한 스트림 언제사용하는 것이 좋은가?
---

코딩테스트를 준비하면서 `프로그래머스`에서 제공하는 문제들을 다시 풀어보고 있다. [완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576)라는 문제를 풀던 중, 문득 `리팩터링`이라는 책에서 [**반복문을 파이프라인으로 바꿔라**](/posts/refactoring/smell-in-code/#반복분)는 격언이 있었다는 사실이 떠올랐다. 기존에 반복문으로 쓰여진 코드들을 모두 파이프라인으로 변경해보기로 결정했다.

> 파이프라인은 함수형 프로그래밍의 기법 중 하나인데, 데이터 처리를 여러 단계로 처리하는 구조를 말한다.

리팩터링
---

### 반복문으로 짠 코드

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

옛날에 같은 문제를 풀었을 당시에 해결했던 코드는 다음과 같다. `for문`이 `세 번`이나 반복되는 코드인데, 로직 코드가 코드 블럭으로 둘러싸여 있는 탓에 한 눈에 읽히지 않는다.

### 파이프라인으로 짠 코드

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

이번에 문제를 풀 때는 조언에 따라 파이프라인을 적용하였다. 모든 반복문을 `Stream`으로 변경하였는데, 실제로 코드가 간결해졌고 읽기 쉬운 코드가 되었다.

파이프라인의 장점은 데이터를 `어떻게(how)` 조작하는가가 아니라 `어떤(what)` 작업을 하는가에 포커싱을 하여 **코드의 의도가 잘 드러난다**는 데에 있다는 것을 이번 문제 풀이를 통해서 알 수 있었다.


항상 좋은 건 아니다?
---

`리팩터링`이나 `클린코드`에서는 퍼포먼스 측면도 중요하지만, **결국 사람이 읽기 쉬운 코드가 더 좋은 코드**라고 했다. 그렇게 생각하면 파이프라인으로 짜여진 코드는 장점밖에 없지 않는가? 동일한 작업을 가독성 좋은 코드로 짤 수 있다니. 하지만 내 생각과는 다르게 인터넷에는 부정적인 내용들이 있었다. 

한 글에서는 퍼포먼스 측면에서 `반복문`과 `Stream`의 극적인 차이가 있기 때문에 사용을 자제해야 한다고 했다. 실제로 나는 반복문으로 짜여진 코드와 파이프라인을 이용한 코드의 속도 차이가 얼마나 나는지가 궁금했고, 그래서 두 코드의 속도 차이를 비교해보았다.

|  | 반복문 | 파이프라인 |
|--------|--------|------------|
| 테스트 1 | 1.58ms | 1.42ms |
| 테스트 2 | 2.17ms | 2.16ms |
| 테스트 3 | 2.59ms | 2.29ms |
| 테스트 4 | 7.06ms | 3.18ms |
| 테스트 5 | 2.89ms | 3.13ms |
| 테스트 6 | 55.80ms | 41.25ms |
| 테스트 7 | 83.86ms | 95.49ms |
| 테스트 8 | 82.06ms | 109.19ms |
| 테스트 9 | 106.20ms | 134.83ms |
| 테스트 10 | 119.71ms | 81.35ms |

결과적으로 어떤 것을 사용하든 `비슷한 실행시간`이 나왔다. 아니 때로는 오히려 파이프라인을 사용할 때 더 좋은 퍼포먼스를 보여주기도 했다.

결론
---

앞선 코드 실행 결과를 두고보면 그렇게 걱정하지 않아도 될 수준이다. 자바8이 나온 당시에는 함수형 프로그래밍의 기법들이 적용된 시점이었기에 최적화에 문제가 있었을 지도 모르겠다. 하지만 지금은 자바16까지 나온만큼 많은 최적화를 거쳤을 것이다.

무엇보다 퍼포먼스만을 위해서 가독성을 포기하는 것은 유지보수 측면에서 `더 큰 비용`을 야기할 수 있다. 일반적인 경우에는 파이프라인을 통해 기능을 개발하되, `크리티컬한 성능 저하`가 있는 경우에 반복문을 사용하면 해결될 일이다.
