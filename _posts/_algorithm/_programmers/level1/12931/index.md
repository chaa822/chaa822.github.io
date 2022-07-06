---
title: Programmers - 자릿수 더하기
date: 2022-03-10
tags:
- Algorithm
keywords:
- Programmers
- 프로그래머스
- 자릿수 더하기
---

문제 [링크](https://school.programmers.co.kr/learn/courses/30/lessons/12931)

![](screenshot.png)

_**Java 풀이**_

#### 1번
```java
import java.util.*;

public class Solution {
    public int solution(int n) {
        int answer = 0;
        while( n > 0 ){
            answer += n % 10;
            n = n / 10;
        }
        return answer;
    }
}
```

#### 2번
```Java
import java.util.*;

public class Solution {
    public int solution(int n) {
        int answer = 0;
        String s = String.valueOf(n);    
        for(char c : s.toCharArray()){
            int num = c - '0';
            answer += num;
        }
        return answer;
    }
}
```
