---
title: Programmers - 주식가격
date: 2022-03-21
tags:
- Algorithm
keywords:
- Programmers
- 프로그래머스
- 주식가격
---

문제 [링크](https://school.programmers.co.kr/learn/courses/30/lessons/42584)

![](img.png)

_**Java 풀이**_
```java
class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];        
        for(int index = 0; index < prices.length; index++){
            int second = 0;
            for(int index2 = index + 1; index2 < prices.length; index2++){
                second = second + 1;
                if( prices[index] > prices[index2] ) break;                
            }
            answer[index] = second;
        }
        return answer;
    }
}
```
