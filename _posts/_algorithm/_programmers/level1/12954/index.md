---
title: Programmers - x만큼 간격이 있는 n개의 숫자 (Java)
date: 2022-03-08
tags:
- Algorithm
keywords:
- Programmers
- 프로그래머스
- x만큼 간격이 있는 n개의 숫자
---

문제 [링크](https://school.programmers.co.kr/learn/courses/30/lessons/12954)

![](problem.png)

_**Java 풀이**_
```java
class Solution {
    public long[] solution(int x, int n) {
        
        long temp = 0;
        long[] answer = new long[n];
        
        for(int i = 0; i < n; i++){
            answer[i] = temp = temp + x;
        }
        
        return answer;
    }
}
```
