---
title: leetcode(리트코드)-1742 Maximum Number of Balls in a Box(PYTHON)
author: 강민석
date: 2021-08-04 04:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1742 - Maximum Number of Balls in a Box  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-number-of-balls-in-a-box/> 

![](/assets/img/sample/leetcode/1742/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1742/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- lowLimit과 highLimit이 주어집니다.
- lowLimit부터 highLimit까지 적힌 숫자공이 있다고 가정합니다.
- 각자리 숫자의 합에 해당하는 박스에 공을 넣는다고 가정할 때 (16이면 1 + 6 = 7인 박스) 공이 가장 많긴 박스의 공의 개수를 리턴하세요.




-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def countBalls(self, lowLimit: int, highLimit: int) -> int:
        res = [0] * 100
        ans = 0
        for i in range(lowLimit,highLimit+1):
            #brute
            temp = str(i)
            sumnum = 0 
            for k in range(len(temp)):
                sumnum += int(temp[k])
            res[sumnum] += 1 
            ans = max(ans,res[sumnum])
        return ans
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1742/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


