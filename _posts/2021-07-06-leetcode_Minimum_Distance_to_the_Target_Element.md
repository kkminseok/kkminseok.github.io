---
title: leetcode(리트코드)-1848 Minimum Distance to the Target Element(python)
author: 강민석
date: 2021-07-06 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1848 - Minimum Distance to the Target Element  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-distance-to-the-target-element/> 

![](/assets/img/sample/leetcode/1848/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1848/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- target으로 들어오는 값을 찾아 start와의 절댓값 뺄셈이 가장 작은 값을 리턴하세요.
- 일반적으로 푸나 탐색을 start에서 시작하나 수행시간이 크게 다르지 않습니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def getMinDistance(self, nums: List[int], target: int, start: int) -> int:
        left = start
        right =start
        while 1:
            if nums[left] == target : 
                return abs(left - start)
            elif nums[right] == target : 
                return right-start
            if left >0 : 
                left-=1
            if right+1 < len(nums):
                right+=1
                
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1848/result.JPG)  

필요시 c++로 짜드리겠습니다.



