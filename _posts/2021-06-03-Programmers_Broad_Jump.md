---
title: Programmers_연습문제 멀리 뛰기(python)
author: 강민석
date: 2021-06-03 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -멀리 뛰기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12914>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 비슷한 유형의 문제가 많습니다. DP로 해결할 수 있습니다.
- DP[i-1] + 1(점프)을 하는 경우와 DP[i-2] + 2(점프)하는 경우로 나누엇 생각하면 DP[i] = DP[i-1] + DP[i-2] 라는 점화식이 나옵니다.







-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(n):
    mod = 1234567
    DP =[0] * (n+1)
    DP[1] = 1
    if n == 1 : 
        return 1
    DP[2] = 2
    for i in range(3,n+1) : 
        DP[i] = (DP[i-1] + DP[i-2])%mod
    return DP[n]%mod
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
