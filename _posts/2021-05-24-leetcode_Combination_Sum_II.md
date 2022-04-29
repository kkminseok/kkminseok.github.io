---
title: leetcode(리트코드)-40 Combination Sum II(python)
author: 강민석
date: 2021-05-24 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 40 - Combination Sum II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/combination-sum-ii/> 

![](/assets/img/sample/leetcode/40/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/40/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.

-----  

## 4. 문제 해석

- 주어진 리스트의 요소를 통해 target을 만들어야합니다.
- 만들 수 있는 경우의 수를 리스트에 넣어 반환합니다.
- DFS를 이용하면 풀 수 있습니다.



-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        self.DFS(sorted(candidates),target,0,[],res)
        return res
    def DFS(self, nums,target,idx,path,res):
        if target == 0 : 
            res.append(path)
        elif target < 0 :
            return
        for i in range(idx,len(nums)):
            #중복 처리
            if i > idx and nums[i] == nums[i-1]:
                continue
            self.DFS(nums,target-nums[i],i+1,path + [nums[i]],res)
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/40/result.JPG)  

필요시 c++로 짜드리겠습니다.



