---
title: leetcode(리트코드)-137 Single Number II(PYTHON)
author: 강민석
date: 2021-07-19 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 137 - Single Number II  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/single-number-ii/> 

![](/assets/img/sample/leetcode/137/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/137/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다.
- nums에 한 값만 빼고 3번씩 반복된다고 할 때 반복되지 않는 한 요소를 리턴하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        #하나 빼고 전부 3번 반복될 때 나머지 1개
        dic = {}
        for i in range(len(nums)):
            if nums[i] not in dic : 
                dic[nums[i]]=1
            else : 
                dic[nums[i]]+=1
                if dic[nums[i]] == 3 :
                    del[dic[nums[i]]]
        res = list(dic.keys())
        return res[0]
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/137/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


