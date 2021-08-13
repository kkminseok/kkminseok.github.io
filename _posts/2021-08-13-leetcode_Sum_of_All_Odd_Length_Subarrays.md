---
title: leetcode(리트코드)-1588 Sum of All Odd Length Sumarrays(PYTHON)
author: 강민석
date: 2021-08-13 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1588 - Sum of All Odd Length Subarrays  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sum-of-all-odd-length-subarrays/> 

![](/assets/img/sample/leetcode/1588/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1588/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- arr가 주어집니다.
- 모든 연속된 인덱스를 홀수번 선택하여 더한 값을 리턴하세요.




-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def sumOddLengthSubarrays(self, arr: List[int]) -> int:
        res = 0 
        for i in range(len(arr)):
            window = 0
            while i + window < len(arr) : 
                res += sum(arr[i:i+window+1])
                window+=2
        return res
                    
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1588/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


