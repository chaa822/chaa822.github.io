---
title: Programmers - 정수 제곱근 판별
date: 2022-03-10
tags:
- Algorithm
  keywords:
- Programmers
- 프로그래머스
- 정수 제곱근 판별
---

문제 [링크](https://school.programmers.co.kr/learn/courses/30/lessons/12934)

![](img.png)

_**Java 풀이**_
```java
import java.util.*;

class Solution {
    public long solution(long n) {
        long answer = -1;
        double result = Math.sqrt(n);
        int num = (int) result;
        if( result == num )
            answer = (long) (num + 1) * (num + 1);
        return answer;
    }
}
```
