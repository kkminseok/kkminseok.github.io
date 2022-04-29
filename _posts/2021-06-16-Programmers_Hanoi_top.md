---
title: Programmers_연습문제 하노이 탑(python)
author: 강민석
date: 2021-06-16 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -하노이 탑 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12946>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 하노이탑은 유명하여 자세하게 설명이 나온 내용들이 많으므로 생략하겠습니다.


참고 : <https://shoark7.github.io/programming/algorithm/tower-of-hanoi>

-----  

## 4. 접근 방법을 적용한 코드

```python
def hanoi(n,start,end,between,answer) : 
    if n == 1:
        answer.append([start,end])
    else : 
        hanoi(n-1,start,between,end,answer)
        answer.append([start,end])
        hanoi(n-1,between,end,start,answer)
def solution(n):
    answer = []
    hanoi(n,1,3,2,answer)
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
