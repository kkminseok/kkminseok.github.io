---
title: leetcode(리트코드)-598 Range Addition II(PYTHON)
author: 강민석
date: 2021-08-31 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 598 - Range Addition II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/range-addition-ii/> 

![](/assets/img/sample/leetcode/598/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/598/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- input으로 m * n 배열이 들어옵니다.
- ops배열에서 각 요소의 첫번째요소와 두번째 요소의 의미는 해당 인덱스까지 1씩더한다는 의미입니다. [2,2]는 [1,1], [1,2],[2,1],[2,2]까지 1씩 더하라는 의미입니다.





-----  

## 5. code  



**python**

```python
class Solution:
    def maxCount(self, m: int, n: int, ops: List[List[int]]) -> int:
        if len(ops) == 0 :
            return m * n
        minrow = 10**5
        mincol = 10**5
        for i in range(len(ops)):
            minrow = min(ops[i][0],minrow)
            mincol = min(ops[i][1],mincol)
        return minrow * mincol
    
        

        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/598/result.JPG)  



필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


