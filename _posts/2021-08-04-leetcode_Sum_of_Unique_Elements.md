---
title: leetcode(리트코드)-1748 Sum of Unique Elements(PYTHON)
author: 강민석
date: 2021-08-04 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1748 - Sum of Unique Elements  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sum-of-unique-elements/> 

![](/assets/img/sample/leetcode/1748/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1748/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다.
- 2번 이상 중복되지 않는 요소에 대하여 값들을 더해 더한 값을 리턴하세요.




-----  

## 5. code  

코드설명
- 데이터가 크지 않아서 밑의 방식으로 해결할 수 있습니다.



**python**

```python
class Solution:
    def sumOfUnique(self, nums: List[int]) -> int:
        res = [0] * 101
        ans = 0
        for i in range(len(nums)):
            res[nums[i]] +=1
        for i in range(len(res)):
            if res[i]== 1 :
                ans+=i
        return ans
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1748/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


