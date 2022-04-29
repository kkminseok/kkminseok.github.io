---
title: leetcode(리트코드)-611 Valid Triangle Number(PYTHON)
author: 강민석
date: 2021-07-16 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 611 - Valid Triangle Number  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/valid-triangle-number/> 

![](/assets/img/sample/leetcode/611/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/611/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums에 3개를 요소를 골랐을 때 그 요소들로 삼각형을 만들 수 있으면 갯수를 셉니다.
- 최대 갯수를 리턴하세요.

-----  

## 5. code  

코드설명

**PYTHON**

```python
class Solution:
    def triangleNumber(self, nums: List[int]) -> int:
        nums.sort()
        size = len(nums)
        ans = 0
        for k in range(2,size) : 
            i = 0
            j = k-1
            while i < j :
                if nums[i] + nums[j] > nums[k] : 
                    ans += j - i 
                    j-=1
                else:
                    i+=1
        return ans
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/611/result.JPG)  





