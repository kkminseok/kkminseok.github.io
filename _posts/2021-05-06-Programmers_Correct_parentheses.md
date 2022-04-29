---
title: Programmers_올바른 괄호
author: 강민석
date: 2021-05-06 12:32:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -올바른 괄호 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12909>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 스택을 이용하면 간단하게 풀 수 있습니다.




-----  

## 4. 접근 방법을 적용한 코드


```python
from collections import deque
def solution(s):
    stack = deque()
    for i in range (len(s)):
        if s[i] =='(':
            stack.append('(')
        else :
            if len(stack) == 0 :
                return False
            stack.popleft()
            
    if stack : 
        return False
    return True
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
