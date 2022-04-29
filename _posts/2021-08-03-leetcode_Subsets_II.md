---
title: leetcode(리트코드)-90 Subsets_II(PYTHON)
author: 강민석
date: 2021-08-03 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 90 - Subsets_II  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/subsets-ii/> 

![](/assets/img/sample/leetcode/90/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/90/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums가 주어집니다.
- 정렬했을 때 중복되지 않는 subsets들을 구해 리스트에 담아 리턴합니다.




-----  

## 5. code  

코드설명
- subset을 구하고 정렬해서 set()에 있는 지 확인합니다.


**python**

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        self.result = []
        self.s = set()
        self.solution(0,nums,[])
        return self.result
    
    def solution(self, depth : int, nums : List[int], temp : List[int]):
        if depth == len(nums):
            temp.sort()
            temp = tuple(temp)
            if temp not in self.s : 
                self.result.append(list(temp))
                self.s.add(temp)
            return
        temp.append(nums[depth])
        self.solution(depth+1,nums,temp[:])
        temp.pop()
        self.solution(depth+1,nums,temp[:])
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/90/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


