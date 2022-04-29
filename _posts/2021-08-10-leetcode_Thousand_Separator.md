---
title: leetcode(리트코드)-1556 Thousand Separtator(PYTHON)
author: 강민석
date: 2021-08-10 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1556 - Thousand Separtator  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/thousand-separator/> 

![](/assets/img/sample/leetcode/1556/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1556/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n이 주어집니다.
- 금액에서 천단위로 '.'을 찍으시오.



-----  

## 5. code  

코드설명



**python**

```python
class Solution:
    def thousandSeparator(self, n: int) -> str:
        n = str(n)
        res = ""
        for i in range(len(n)-1, -1,-1) :
            if (len(n)- i - 1) %3 == 0 :
                    res+= "."
            res+=n[i]
        return res[::-1][:-1]
                   
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1556/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


