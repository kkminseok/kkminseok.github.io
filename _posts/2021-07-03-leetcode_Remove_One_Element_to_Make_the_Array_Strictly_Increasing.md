---
title: leetcode(리트코드)-1909 Remove One Element to Make the Array Strickly Increasing(python)
author: 강민석
date: 2021-07-03 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1909 - Remove One Element to Make the Array Strictly Increasing  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-one-element-to-make-the-array-strictly-increasing/> 

![](/assets/img/sample/leetcode/1909/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1909/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 들어옵니다. 해당 문자열에서 1개만 지웠을 때 오름차순으로 정렬된 리스트로 만들 수 있는 지 확인해야합니다.
- 2가지 상태가 있습니다. 일단 i-1(이전 요소)보다 i가 작은 경우에서 i-2보다도 작은 경우라면 False입니다. 이전요소보다 i가 작은 경우에는 i를 i-1로 갱신해주면 됩니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def canBeIncreasing(self, nums: List[int]) -> bool:
        res = 0 
        for i in range(1,len(nums)):
            if nums[i-1] >= nums[i] : 
                res+=1
                if i >1 and nums[i-2] >= nums[i] : 
                    nums[i] = nums[i-1]
        return res < 2  
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1909/result.JPG)  

필요시 c++로 짜드리겠습니다.



