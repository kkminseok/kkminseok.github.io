---
title: Programmers_연습문제 거스름돈(python)
author: 강민석
date: 2021-06-03 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -거스름돈 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12907>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 백준의 동전1이 똑같은 문제입니다.
- DP로 풀면 해결할 수 있습니다.
- 문제의 핵심은 동전의 갯수가 중심이라서, 그 부분을 찾아 1씩 추가해주는 것입니다.







-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(n, money):
    money.sort()
    mod = 1000000007
    DP = [0] * (n+1)
    DP[0] = 1
    for j in range(len(money)):
        for i in range(1,n+1):
            if i-money[j] >=0: 
                DP[i] +=( DP[i-money[j]])%mod
              
    return DP[n]%mod
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
