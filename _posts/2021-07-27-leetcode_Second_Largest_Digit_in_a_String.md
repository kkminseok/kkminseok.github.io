---
title: leetcode(리트코드)-1796 Second Largest Digit in a String(PYTHON)
author: 강민석
date: 2021-07-27 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1796 - Second Largest Digit in a String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/second-largest-digit-in-a-string/> 

![](/assets/img/sample/leetcode/1796/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1796/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s가 주어집니다.
- s안에 숫자들이 주어집니다.
- 숫자 중에서 2번째로 큰 값을 찾아서 리턴하세요.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def secondHighest(self, s: str) -> int:
        check = [0] * 10
        for i in range(len(s)):
            if s[i].isdigit():
                check[ord(s[i]) -48] +=1
        first = False
        for i in range(len(check)-1,-1,-1):
            if check[i] != 0 :
                if first == False : 
                    first = True
                else : 
                    return i
        return -1
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1796/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


