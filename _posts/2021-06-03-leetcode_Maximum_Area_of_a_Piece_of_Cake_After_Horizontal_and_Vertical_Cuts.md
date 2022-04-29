---
title: leetcode(리트코드)6월03일 challenge1465-Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts(python)
date: 2021-06-03 02:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 03일 - Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-area-of-a-piece-of-cake-after-horizontal-and-vertical-cuts/>  

![](/assets/img/sample/leetcode/1465/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1465/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 03일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- h,w의 값을 맵을 그려, 수직선과 수평선 값들이 들어옵니다.
- 들어온 수직선과 수평선을 기준으로 맵을 자른다고 생각했을 때 자른 사각형중 가장 넓이가 큰 값의 사각형의 넓이를 리턴하세요.




-----  

## 5. code

- 첫 부분과 끝 부분에도 사각형이 자동으로 생기므로 그 좌표값인 (0,0), (h,w)를 추가해주었습ㄴ니다.
- 수직선과 수평선을 돌면서 다음 값의 차이가 가장 큰 값이 해당 길이가 됩니다.


**python**

```python
class Solution:
    def maxArea(self, h: int, w: int, ho: List[int], ve: List[int]) -> int:
        ho.append(0)
        ho.append(h)
        ve.append(0)
        ve.append(w)
        ho.sort()
        ve.sort()
        maxrow = 0
        maxcol = 0
        for i in range(1,len(ho)):
            if ho[i] - ho[i-1] > maxrow : 
                maxrow = ho[i]-ho[i-1]
        for i in range(1,len(ve)):
            if ve[i] - ve[i-1] > maxcol : 
                maxcol = ve[i] - ve[i-1]
        return (maxrow*maxcol)%(pow(10,9)+7)
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1465/result.JPG)  

필요시 c++로 풀어드립니다.



