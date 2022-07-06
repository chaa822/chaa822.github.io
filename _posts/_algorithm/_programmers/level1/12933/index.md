---
title: Programmers - 정수 내림차순으로 배치하기
date: 2022-03-12
tags:
- Algorithm
keywords:
- Programmers
- 프로그래머스
- 정수 내림차순으로 배치하기
---

문제 [링크](https://school.programmers.co.kr/learn/courses/30/lessons/12933)

![](screenshot.png)

_**Java 풀이**_
```java
import java.util.*;

class Solution {
    public long solution(long n) {
        long answer = 0;
        
        String s = String.valueOf(n);
        char[] arr = s.toCharArray();
        
        Arrays.sort(arr);
        s = new StringBuilder(new String(arr)).reverse().toString();
        
        answer = Long.parseLong(s);
        
        return answer;
    }
}
```
