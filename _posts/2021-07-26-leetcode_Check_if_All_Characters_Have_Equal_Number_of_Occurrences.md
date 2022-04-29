---
title: leetcode(리트코드)-1941 Check if All Characters Have Equal Number of Occurrences(PYTHON)
author: 강민석
date: 2021-07-26 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1941 - Check if All Characters Have Equal Number of Occurrences  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-all-characters-have-equal-number-of-occurrences/> 

![](/assets/img/sample/leetcode/1941/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1941/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s라는 문자열이 들어옵니다.
- 문자열의 각 알파벳이 동일하게 카운트되면 True를 리턴하고 아니면 False를 리턴합니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def areOccurrencesEqual(self, s: str) -> bool:
        dic = {}
        pivot = 0
        for i in range(len(s)):
            if s[i] not in dic : 
                dic[s[i]]=1
            else : 
                dic[s[i]]+=1
            pivot = dic[s[i]]
        for count in dic : 
            if dic[count] != pivot: 
                return False
        return True
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1941/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


