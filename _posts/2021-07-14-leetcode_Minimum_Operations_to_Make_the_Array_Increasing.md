---
title: leetcode(리트코드)-1827 Minumum Operations to Make the Array Increasing(SQL)
author: 강민석
date: 2021-07-14 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1827 - Minimum Operations to Make the Array Increasing  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-operations-to-make-the-array-increasing/> 

![](/assets/img/sample/leetcode/1827/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1827/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다. 해당 nums를 오름차순으로 만들어야합니다.
- 각 요소를 1씩 올릴 수만 있을 때 최소한의 방법으로 오름차순을 만들 수 있게하세요.
- 1을 올린 카운트를 리턴하세요.

-----  

## 5. code  

코드설명

**PYTHON**

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        res = 0 
        if len(nums) == 1 : 
            return 0
        else : 
            for i in range(1,len(nums)):
                if nums[i] <= nums[i-1] : 
                    res += nums[i-1] - nums[i]+1
                    nums[i] = nums[i-1] + 1
        return res
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1827/result.JPG)  





