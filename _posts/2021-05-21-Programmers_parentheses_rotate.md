---
title: Programmers_월간 코드 챌린지 시즌2 괄호 회전하기(python)
author: 강민석
date: 2021-05-21 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -괄호 회전하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/76502>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌2 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- brute하게 풀었지만, brute하게 접근을 안하는 방법이 있습니다.
- 올바른 괄호를 찾고 그 괄호에서의 올바른 괄호쌍들의 갯수를 찾아주면 되겠지만, brute하게 풀어도 시간초과가 나지 않으므로 brute하게 해결했습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python

from collections import deque
def solution(s):
    answer = 0
    for i in range(len(s)):
        temp  = s[i:]+ s[:i]
        ls = deque()
        check = True
        for j in range(len(temp)):
            ct = temp[j]
            if ct =="(" or ct =="[" or ct =="{" :
                ls.append(ct)
            else :
                if len(ls) == 0 :
                    check = False
                    break
                popct = ls.pop()
                if popct == "(" and ct != ")" : 
                    check = False
                    break
                elif popct =="{" and ct != "}":
                    check = False
                    break
                elif popct =="[" and ct !="]":
                    check = False
                    break
        if check and len(ls)==0:
           answer+=1 
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
