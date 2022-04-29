---
title: leetcode(리트코드)-1903 Largest Odd Number in String(python)
author: 강민석
date: 2021-06-20 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1903 - Largest Odd Number in String 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/largest-odd-number-in-string/> 

![](/assets/img/sample/leetcode/1903/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1903/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열로 숫자가 들어옵니다. 각 substring에서 가장 길고 홀수인 substring을 찾아 리턴하세요.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def largestOddNumber(self, num: str) -> str:
        for i in range(len(num)-1, -1, -1):
            if int(num[i]) % 2 == 1 : 
                return num[0:i+1]
        return ""
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1903/result.JPG)  

필요시 c++로 짜드리겠습니다.



