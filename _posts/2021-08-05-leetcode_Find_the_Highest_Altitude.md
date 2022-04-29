---
title: leetcode(리트코드)-1732 Find the Highest Altitude(PYTHON)
author: 강민석
date: 2021-08-05 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1732 - Find the Highest Altitude  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-the-highest-altitude/> 

![](/assets/img/sample/leetcode/1732/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1732/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- gain이 주어집니다.
- 주어진 숫자를 더해 나갈 때 가장 큰 값을 리턴하세요. 
- 단, 0보다는 커야합니다. 0보다 작을 경우 0을 리턴합니다.




-----  

## 5. code  

코드설명



**python**

```python
class Solution:
    def largestAltitude(self, gain: List[int]) -> int:
        res = 0 
        tmpsum = 0
        for i in range(len(gain)):
            tmpsum += gain[i]
            res = max(tmpsum,res)
        return res
            
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1732/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


