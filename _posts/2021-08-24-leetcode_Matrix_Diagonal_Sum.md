---
title: leetcode(리트코드)-1572 Matrix Diagonal Sum(PYTHON)
author: 강민석
date: 2021-08-24 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1572 - Matrix Diagonal Sum  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/matrix-diagonal-sum/> 

![](/assets/img/sample/leetcode/1572/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1572/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- matrix가 주어집니다.
- 대각선으로 합한 결과를 구하세요. 단, 중복은 제외합니다.





-----  

## 5. code  

코드설명



**python**

```python
class Solution:
    def diagonalSum(self, mat: List[List[int]]) -> int:
        sz = len(mat)
        res = 0
        for i in range(sz) : 
            res += mat[i][i] + mat[i][sz-i-1]
        return res if sz % 2 == 0 else res - mat[sz//2][sz//2]
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1572/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


