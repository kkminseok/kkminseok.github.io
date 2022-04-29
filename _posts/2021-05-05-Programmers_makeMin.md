---
title: Programmers_최솟값 만들기
author: 강민석
date: 2021-05-05 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -최솟값 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12941>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다. 데이터도 크게 들어오지 않습니다.
- 최소값을 만들려면 하나는 가장 작은 갑을 뽑고 다른 하나에서 가장 큰 값을 뽑아 곱해버리면 된다고 생각했습니다.



-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(A,B):
    answer = 0
    A.sort()
    B.sort()
    size = len(A)-1
    for i in range (size+1):
        answer += (A[i] * B[size-i])
    return answer
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
