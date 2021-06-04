---
title: leetcode(리트코드)-119 Pascal's Triangle II(python)
author: 강민석
date: 2021-06-04 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 119 - Pascal's Triangle II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/pascals-triangle-ii/> 

![](/assets/img/sample/leetcode/119/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/119/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 파스칼 삼각형을 구현하여, 문제에서 주어진 rowIndex에 해당하는 행의 값들을 리턴합니다.



-----  

## 5. code  

코드설명

**python**

```python

class Solution:
    def getRow(self, rowIndex: int) -> List[int]:
        DP =[0] * (rowIndex+1)
        for i in range(len(DP)):
            DP[i] = [0] * (i+1)
        DP[0][0] = 1
        for i in range(rowIndex+1) : 
            for j in range(i+1) :
                if j ==0 or j == i : 
                    DP[i][j] = 1
                else : 
                    DP[i][j] = DP[i-1][j-1] + DP[i-1][j]
    
        return DP[rowIndex]
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/119/result.JPG)  

필요시 c++로 짜드리겠습니다.



