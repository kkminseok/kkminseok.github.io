---
title: Programmers_Summer/Winter Coding(~2018) 점프와 순간이동(python)
author: 강민석
date: 2021-05-23 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -점프와 순간이동 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12980>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer/Winter Coding(~2018) 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 역으로 생각하면 어렵지 않습니다. N에서 0으로 만들어야하는데, 홀수인경우는 순간이동으로 온 것이 아니므로 -1을 해주면서 자원소모를 해줍니다.

-----  

## 4. 접근 방법을 적용한 코드


```python

def solution(n):
    ans = 0
    while(n!=0) : 
        if n%2 ==1 : 
            ans+=1
            n-=1
        else : 
            n = n//2
    return ans
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
