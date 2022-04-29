---
title: leetcode(리트코드)-1791 Find Center of Star Graph(PYTHON)
author: 강민석
date: 2021-07-29 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1791 - Find Center of Star Graph  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-center-of-star-graph/> 

![](/assets/img/sample/leetcode/1791/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1791/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 그래프가 주어졌을 때 중앙값을 찾아 리턴하세요.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def findCenter(self, edges: List[List[int]]) -> int:
        dic ={}
        n  = len(edges)
        for i in range(len(edges)):
            first,second = edges[i]
            if first not in dic : 
                dic[first] = 1
            else : 
                dic[first] += 1
            if second not in dic :
                 dic[second] = 1
            else : 
                dic[second] += 1
        for i in dic :
            if dic[i] == n : 
                return i
            
        
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1791/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


