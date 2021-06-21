---
title: leetcode(리트코드)-1886 Determine Whether Matrix Can Be Obtained By Rotation Equal(python)
author: 강민석
date: 2021-06-20 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1886 - Determine Whether Matrix Can Be Obtained By Rotation 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/determine-whether-matrix-can-be-obtained-by-rotation/> 

![](/assets/img/sample/leetcode/1886/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1886/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 숫자가 들어있는 정사각형이 주어집니다. 정사각형을 90도씩 돌렸을 때 target과 일치하면 True 아니면 False를 리턴합니다.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def findRotation(self, mat: List[List[int]], target: List[List[int]]) -> bool:
        def rotate(mat):
            new = copy.deepcopy(mat)
            for i in range(len(mat)):
                for j in range(len(mat[i])):
                    new[j][len(mat[i]) - i - 1] = mat[i][j] 
            return new
        for i in range(4):
            if mat == target : 
                return True
            mat = rotate(mat)
        return False
        
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1886/result.JPG)  

필요시 c++로 짜드리겠습니다.



