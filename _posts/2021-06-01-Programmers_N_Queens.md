---
title: Programmers_연습문제 N-Queen(python)
author: 강민석
date: 2021-06-01 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -N-Queen 타일링 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12952>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 유명한 문제입니다.
- leetcode에서는 Hard로 분류되어있고, 효율성까지 따지는 문제라 Level3 문제는 아닌 것 같은데..




-----  

## 4. 접근 방법을 적용한 코드

```python
def promising(i,col):
            k = 0
            switch = True
            while k < i and switch == True :
                if col[i] == col[k] or abs(col[i] - col[k]) == i -k :
                    switch = False
                k+=1
            return switch
def queens(n,i,col,res): 
    if promising(i,col) : 
        if i == n-1 :
            size = len(res)
            res.append([])
            for j in range(len(col)):
                find = col[j]
                res[size].append(1)
        else:
            for j in range(0,n):
                col[i+1] = j
                queens(n,i+1,col,res)

def solution(n):
    col = n *[0]
    res = []
    queens(n,-1,col,res)
    return len(res)
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
