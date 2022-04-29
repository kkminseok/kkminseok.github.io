---
title: leetcode(리트코드)-1752 Check if Array Is Sorted and Rotated(PYTHON)
author: 강민석
date: 2021-08-04 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1752 - Check if Array Is Sorted and Rotated  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/> 

![](/assets/img/sample/leetcode/1752/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1752/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다.
- nums를 회전했을 때 오름차순으로 정렬되어 있으면 True를 회전을 아무리 해도 오름차순으로 정렬될 수 없다면 False를 리턴합니다.




-----  

## 5. code  



**python**

```python
class Solution:
    def check(self, nums: List[int]) -> bool:
        temp = nums[:]
        temp.sort()
        nums = deque(nums)
        temp = deque(temp)
        for i in range(len(nums)):
            if temp == nums:
                return True
            else:
                shift = nums.popleft()
                nums.append(shift)
            
        return False
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1752/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


