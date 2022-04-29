---
title: Programmers_2018 KAKAO BLINE RECURITMENT[3차] n 진수 게임(python)
author: 강민석
date: 2021-05-28 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -n진수 게임 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17687?language=python3>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2018 KAKAO BLIND RECURITMENT 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- DP로 풀까하다가 힘들것 같아서brute하게 풀었습니다.





-----  

## 4. 접근 방법을 적용한 코드

```python
def makestr(i,n) : 
    res = ""
    nums = "0123456789ABCDEF"
    while i :
        res=  nums[i%n] + res
        i=i//n
    return res
def solution(n, t, m, p):
    answer = ''
    #for문을 돌면서 원하는 진법으로 만들어주는데 0이 들어갈 경우 위 함수의 while문을 돌지 않으므로 따로 추가해줍니다.
    binarystr = "0"
    for i in range(t*m) : 
        binarystr += makestr(i,n)
    p = p -1
    for i in range(t) : 
        answer+=(binarystr[p])
        p +=m
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
