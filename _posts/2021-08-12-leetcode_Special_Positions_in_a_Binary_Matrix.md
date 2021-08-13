---
title: leetcode(리트코드)-1582 Speical Positions in a Binary Matrix(PYTHON)
author: 강민석
date: 2021-08-12 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1582 - Speical Positions in a Binary Matrix  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/special-positions-in-a-binary-matrix/> 

![](/assets/img/sample/leetcode/1582/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1582/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- '0'과 '1'로 이루어진 매트릭스가 주어집니다.
- '1'에 대해서 해당 열과 해당 행에서 유일하게 '1'로 존재한다면 이러한 '1'의 갯수를 찾아 리턴합니다.




-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def numSpecial(self, mat: List[List[int]]) -> int:
        def checkj(mat,j):
            i = 0 
            sumres = 0 
            while i < len(mat) : 
                if mat[i][j] == 1 :
                    sumres += 1
                    if sumres > 1 : 
                        return 2
                i += 1
            return 1
                
        row = len(mat)
        col = len(mat[0])
        res = 0
        for i in range(row):
            for j in range(col):
                if mat[i][j] == 1 and sum(mat[i]) == 1 and checkj(mat,j) == 1 :
                    res += 1
        return res
                    
                    
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1582/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


