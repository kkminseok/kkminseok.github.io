---
title: leetcode(리트코드)-914 X of Kind in a Deck of Cards(PYTHON)
author: 강민석
date: 2021-08-13 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 914 - X of Kind in a Deck of Cards  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/x-of-a-kind-in-a-deck-of-cards/> 

![](/assets/img/sample/leetcode/914/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/914/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- deck이 주어집니다.
- 같은 숫자끼리 분할해야하는데 분할한 자료구조의 요소갯수가 같도록 분할할 수 있으면 True 없다면 False를 리턴합니다.




-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def hasGroupsSizeX(self, deck: List[int]) -> bool:
        def gcd(a, b):
            while b: a, b = b, a % b
            return a
        count = collections.Counter(deck).values()
        #print(count)
        return reduce(gcd, count) > 1
                    
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/914/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


