---
title: leetcode(리트코드)-1952 Three Divisors(PYTHON)
author: 강민석
date: 2021-08-02 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1952 - Three Divisors  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/three-divisors/> 

![](/assets/img/sample/leetcode/1952/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1952/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n이 들어옵니다.
- n이 3개의 약수를 가지면 True 아니면 False를 리턴하세요.



-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def isThree(self, n: int) -> bool:
        count = 0 
        for i in range(1,n+1):
            if n % i == 0 :
                count+=1
                if count >3 :
                    return False
        return True if count == 3 else False
            
                            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1952/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


