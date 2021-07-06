---
title: leetcode(리트코드)-1863 Sum of All Subset XOR Totals(python)
author: 강민석
date: 2021-07-03 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1863 - Sum of All Subset XOR Totals  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sum-of-all-subset-xor-totals/> 

![](/assets/img/sample/leetcode/1863/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1863/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums의 모든 요소들끼리 XOR를 한 결과를 더해 리턴합니다.
- 재귀연습하기에 좋습니다. 재귀모르면.. 어케풀지?
- 어려워서 2주일 뒤에 다시 풀었습니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def subsetXORSum(self, nums: List[int]) -> int:
        def sums(term,idx):
            if idx == len(nums) : 
                return term
            return sums(term,idx+1) + sums(term ^ nums[idx],idx+1)
        return sums(0,0)
        
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1863/result.JPG)  

필요시 c++로 짜드리겠습니다.



