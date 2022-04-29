---
title: Programmers_연습문제 최고의 집합(python)
author: 강민석
date: 2021-06-02 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -최고의 집합 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12938>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 첫 예제에서 어떻게하면 4,5로 나올 지 생각해 봤습니다.
- 또한 n이 3이고 s가 9일 때 3,3,3이 최고의 집합인데, 이 경우의 수가 어떻게 나오는 지 생각해봤습니다.
- 매 번 s에서 n으로 나눈 몫이 최선의 수라고 생각하였고, 코드를 작성하였습니다.





-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(n, s):
    answer = []
    if n >s :
        return [-1]
    while s : 
        nums = s//n
        answer.append(nums)
        n -=1
        s -=nums
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
