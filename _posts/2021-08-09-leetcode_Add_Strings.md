---
title: leetcode(리트코드)-415 Add Strings(PYTHON)
author: 강민석
date: 2021-08-09 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 415 - Add Strings  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/add-strings/> 

![](/assets/img/sample/leetcode/415/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/415/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- num1과 num2이 string의 형태로 들어옵니다.
- num1과 num2를 더한 값을 string형태로 리턴하세요.


-----  

## 5. code  

코드설명




**python**

```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        return str(int(num1) + int(num2))           
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/415/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


