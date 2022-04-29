---
title: leetcode(리트코드)-931 Minimum Falling Path Sum(PYTHON)
author: 강민석
date: 2021-07-30 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 931 - Minimum Falling Path Sum  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-falling-path-sum/> 

![](/assets/img/sample/leetcode/931/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/931/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 파스칼 삼각형처럼 matrix를 순차적으로 내려갑니다.
- 내려가면서 나온 숫자들을 더할 때 가장 작은 값을 리턴하세요.



-----  

## 5. code  

코드설명

- 세 가지 경우가 있습니다.
- col의 사이즈가 1인경우 그냥 흘러내려가면 됩니다.
- col의 사이즈가 2 이상인 경우 위의 폭포의 ↖, ↑, ↗의 경우를 살펴줘야합니다.
- 파이썬의 특성상 범위를 벗어나도 파이썬 자체가 커버를 쳐주는 부분이 있어서 밑의 코드에서 예외처리를 안 해줬습니다.

**python**

```python
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        row = len(matrix)
        col = len(matrix[0])
        if col == 1: 
            res = 0 
            for i in range(row):
                res += matrix[i][0]
            return res
        for i in range(1,row):
            for j in range(col):
                if j == col-1 :
                    matrix[i][j] = min(matrix[i-1][j],matrix[i-1][j-1]) + matrix[i][j]
                elif j != 0 :
                    matrix[i][j] = min(matrix[i-1][j-1],matrix[i-1][j],matrix[i-1][j+1]) + matrix[i][j]
                else : 
                    matrix[i][j] = min(matrix[i-1][j],matrix[i-1][j+1]) + matrix[i][j]
                    
        return min(matrix[row-1])
            
                            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/931/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


