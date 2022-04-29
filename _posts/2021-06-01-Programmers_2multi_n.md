---
title: Programmers_연습문제 2*n 타일링(python)
author: 강민석
date: 2021-06-01 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -2*n 타일링 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12900>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 유명한 문제이므로 자세한 내용은 생략하겠습니다.
- 점화식은 DP[i] = DP[i-1] + DP[i-2]입니다.






-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(n):
    answer = 0
    mod = 1000000007
    DP = [1] * (n+1)
    for i in range(2,n+1) : 
        DP[i] = (DP[i-1] + DP[i-2])%mod
    answer = DP[n]%mod
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
