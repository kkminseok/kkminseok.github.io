---
title: leetcode(리트코드)-1925 Count Square Sum Triples(python)
author: 강민석
date: 2021-07-12 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1925 - Count Square Sum Triples  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-square-sum-triples/> 

![](/assets/img/sample/leetcode/1925/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1925/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n이 들어옵니다. n이 삼각형의 한 변이라고 생각했을 때 n까지의 직각삼각형의 갯수를 구하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def countTriples(self, n: int) -> int:
        dic = {}
        res = 0 
        for i in range(1,n+1) :
            dic[i**2] = 1
        for i in range(n,1,-1) : 
            for j in range(i-1,0,-1):
                if (i**2) - (j**2) in dic : 
                    res+=1
        return res
        
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1925/result.JPG)  

필요시 c++로 짜드리겠습니다.



