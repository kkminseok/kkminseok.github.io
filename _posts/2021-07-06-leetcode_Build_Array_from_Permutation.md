---
title: leetcode(리트코드)-1920 Build Array from Permutation(python)
author: 강민석
date: 2021-07-06 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1920 - Build Array from Permutation  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/build-array-from-permutation/> 

![](/assets/img/sample/leetcode/1920/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1920/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 들어옵니다. 해당 인덱스에 들어있는 숫자를 가진 배열을 리턴해야합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def buildArray(self, nums: List[int]) -> List[int]:
        res = deque()
        for i in nums:
            res.append(nums[i])
        return res
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1920/result.JPG)  

필요시 c++로 짜드리겠습니다.



