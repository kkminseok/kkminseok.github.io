---
title: leetcode(리트코드)-1464 Maximum Product of Two Elements in an Array(PYTHON)
author: 강민석
date: 2021-08-23 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1464 - Maximum Product of Two Elements in an Array  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-product-of-two-elements-in-an-array/> 

![](/assets/img/sample/leetcode/1464/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1464/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- array가 들어옵니다.
- 가장 큰값과 두 번째로 큰 값을 찾아 각 값 - 1을 곱한 결과를 리턴하세요.





-----  

## 5. code  

코드설명

- 정렬해서 요소 두개 곱하기


**python**

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        nums.sort()
        return (nums[-1]-1) * (nums[-2] - 1)
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1464/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


