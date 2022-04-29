---
title: Programmers_N개의 최소공배수
author: 강민석
date: 2021-05-05 13:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - N개의 최소공배수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12953>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다. 데이터도 크게 들어오지 않습니다.

- 유클리드 호제법을 사용해서 문제를 해결했습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
def GCM(a,b):
    while b != 0:
        r = a % b
        a = b
        b = r
    return a
def solution(arr):
    # 최소공배수 공식
    lcm = 0 
    gcm = 0
    # 길이가 1인 배열이 들어올 경우.
    if len(arr)==1:
        return arr[0]
    for i in range(1,len(arr)):
        gcm = GCM(arr[i-1],arr[i])
        lcm = arr[i-1] * arr[i] / gcm
        arr[i] = lcm
    return lcm
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
