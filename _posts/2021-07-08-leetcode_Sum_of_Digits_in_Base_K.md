---
title: leetcode(리트코드)-1837 Sum of Digits in Base K(python)
author: 강민석
date: 2021-07-08 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1837 - Sum of Digits in Base K  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sum-of-digits-in-base-k/> 

![](/assets/img/sample/leetcode/1837/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1837/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n이라는 숫자가 들어옵니다. 
- k진법으로 바꿨을 때 숫자의 각 자리를 더해서 리턴합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def sumBase(self, n: int, k: int) -> int:
        res = ""
        while n :
            res += str(n % k)
            n = n // k
        sumres = 0 
        for i in range(len(res)):
            sumres += int(res[i])
        return sumres
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1837/result.JPG)  

필요시 c++로 짜드리겠습니다.



