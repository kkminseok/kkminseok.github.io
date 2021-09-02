---
title: leetcode(리트코드)-565 Array Nesting(PYTHON)
author: 강민석
date: 2021-09-02 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 565 - Array Nesting 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/array-nesting/> 

![](/assets/img/sample/leetcode/565/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/565/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다.
- 해당 인덱스의 요소로 점프할 수 있다고 할 때 그렇게해서 만들어진 배열의 길이가 가장 긴 배열을 리턴하세요.





-----  

## 5. code  



**python**

```python
        
class Solution:
    def arrayNesting(self, nums: List[int]) -> int:
        res = 0 
        def solution(idx,nums,count) -> int: 
            if nums[idx] == -1 : 
                return count - 1
            nxtidx = nums[idx]
            nums[idx] = - 1
            return solution(nxtidx,nums,count+1)
        for idx,num in enumerate(nums) : 
            count = 0
            if nums[idx] == -1 : 
                continue
            tmpcount = solution(idx,nums,count+1)
            res = max(tmpcount,res)
        return res
        
            
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/565/result.JPG)  



필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


