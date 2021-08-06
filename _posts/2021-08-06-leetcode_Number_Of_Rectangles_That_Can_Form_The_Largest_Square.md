---
title: leetcode(리트코드)-1725 Number Of Rectangles That Can Form The Largest Square(PYTHON)
author: 강민석
date: 2021-08-06 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1725 - Number Of Rectangles That Can Form The Largest Square  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-rectangles-that-can-form-the-largest-square/> 

![](/assets/img/sample/leetcode/1725/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1725/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 

-----  

## 5. code  

코드설명

- 각 리스트의 요소는 사각형의 폭과 높이입니다.
- 변을 잘라서 정사각형을 만들 때 가장 폭이 긴 정사각형의 갯수를 리턴하세요.



**python**

```python
class Solution:
    def countGoodRectangles(self, rectangles: List[List[int]]) -> int:
        res ={}
        maxnum = 0 
        for i in range(len(rectangles)):
            temp = min(rectangles[i][0],rectangles[i][1])
            maxnum = max(maxnum,temp)
            if temp not in res :
                res[temp] = 1
            else :
                res[temp] += 1
        return res[maxnum]
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1725/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


