---
title: Programmers_피보나치 수
author: 강민석
date: 2021-05-05 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 피보나치 수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12945>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 유명한 피보나치 수 문제입니다. 데이터가 크므로 DP로 풀어줘야합니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
#재귀가 많이 호출되므로 최대 재귀 깊이를 늘려줌.
import sys
sys.setrecursionlimit(10**7)
def fibo(n,DP):
    if DP[n]:
        return DP[n]
    if n == 0 or n==1 :
        return 1
    DP[n] = (fibo(n-1,DP) + fibo(n-2,DP)) %1234567
    return DP[n]

def solution(n):
    answer = 0
    DP = [0] * (n + 1)
    fibo(n,DP)
    return DP[n-1] %1234567
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
