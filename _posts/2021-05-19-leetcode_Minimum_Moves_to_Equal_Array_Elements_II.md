---
title: leetcode(리트코드)5월19일 challenge462-Minimum Moves to Equal Array Elements II
date: 2021-05-19 00:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 19일 - Minimum Moves to Equal Array Elements II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii/>  

![](/assets/img/sample/leetcode/462/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/462/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 19일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- nums가 들어옵니다. 하나의 요소를 잡아 다른 요소들을 해당 요소로 바꾸는데 드는 최소 비용을 리턴하세요.
- 한 요소를 1을 더하거나 뺄 수 있습니다.




-----  

## 5. code


**python**

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        nums.sort()
        size = len(nums)
        pick = nums[size//2]
        res = 0 
        for i in range(size) : 
            res += abs(nums[i] - pick)
        return res
```



-----

## 6. 결과 및 후기, 개선점

필요시 c++로 풀어드립니다.




