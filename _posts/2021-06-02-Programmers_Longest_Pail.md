---
title: Programmers_연습문제 가장 긴 펠린드롬(python)
author: 강민석
date: 2021-06-02 11:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -가장 긴 펠린드롬 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12904>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- leetcode 5번이 똑같은 문제입니다.
- 하나의 인덱스를 기준으로 pailndrome을 찾고 확장합니다.






-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(s):
    if len(s) <= 1 :
        return 1
    max_len =1 
    min_left = 0 
    max_right = len(s)-1
    mid = 0 
    while mid < len(s) : 
        left = mid
        right = mid
        while right < max_right and s[right] == s[right+1] : 
            right+=1
            
        mid = right+1
        while right < max_right and left > 0 and s[right+1] == s[left-1] : 
            right+=1
            left-=1
        newlen = right-left+1
        if newlen > max_len : 
            max_len = newlen
    return max_len
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
