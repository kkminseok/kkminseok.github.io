---
title: Programmers_약수의 개수와 덧셈 python
author: 강민석
date: 2021-05-16 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -약수의 개수와 덧셈 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/77884#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌 2 문제입니다.
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제는 직관적으로 어렵지 않습니다.
- 에라토스테네스의 체를 변형하여 코드를 작성하였습니다.





-----  

## 4. 접근 방법을 적용한 코드


```python
import math
def solution(left, right):
    #에라토스테네스의 체 변형
    maxnum = right+1
    tenes = [1] * maxnum
    res = 0
    for i in range(2,maxnum):
        tenes[i] +=1
    for i in range(2,maxnum//2+1) : 
        for j in range(2*i,maxnum,i):
            tenes[j] += 1
    for i in range (left,maxnum) : 
        if tenes[i] %2 == 1 :
            res -= i
        else :
            res += i
    return res
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
