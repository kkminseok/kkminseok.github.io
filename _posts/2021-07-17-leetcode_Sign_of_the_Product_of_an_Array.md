---
title: leetcode(리트코드)-1822 Sign of the Product of an Array(PYTHON)
author: 강민석
date: 2021-07-17 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1822 - Sign of the Product of an Array  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sign-of-the-product-of-an-array/> 

![](/assets/img/sample/leetcode/1822/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1822/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다. 
- 배열의 모든 요소를 곱한 결과가 양수면1, 0이면 0, 음수면 -1을 리턴합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def arraySign(self, nums: List[int]) -> int:
        res = 1
        for i in range(len(nums)):
            if nums[i] == 0 :
                return  0
            elif nums[i] < 0 :
                res *= -1
        return res
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1822/result.JPG)  





