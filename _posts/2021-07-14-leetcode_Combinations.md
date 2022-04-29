---
title: leetcode(리트코드)-77 Combinations(PYTHON)
author: 강민석
date: 2021-07-14 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 77 - Combinations  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/combinations/> 

![](/assets/img/sample/leetcode/77/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/77/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n과 k가 주어집니다. [1,2,3,... ,n]까지의 리스트가 있을 때 k의 combination을 구해 리턴하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        li = [x+1 for x in range(n)]
        li = list(combinations(li, k))
        return li
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/77/result.JPG)  





