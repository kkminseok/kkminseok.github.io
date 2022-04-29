---
title: leetcode(리트코드)-1800 Maximum Ascending Subarray Sum(PYTHON)
author: 강민석
date: 2021-07-27 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1800 - Maximum Ascending Subarray Sum  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-ascending-subarray-sum/> 

![](/assets/img/sample/leetcode/1800/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1800/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다.
- 연속되는 부분배열들의 합중 가장 큰 것을 찾아 그 합을 리턴하세요.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def maxAscendingSum(self, nums: List[int]) -> int:
        DP = [0] * len(nums)
        res = nums[0]
        DP[0] = nums[0]
        for i in range(1,len(nums)):
            if nums[i-1] < nums[i] : 
                DP[i] = DP[i-1] + nums[i]
            else : 
                DP[i] = nums[i]
            res = max(res,DP[i])
        return res
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1800/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


