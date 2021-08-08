---
title: leetcode(리트코드)-1716 Calculate Money in Leetcode Bank(PYTHON)
author: 강민석
date: 2021-08-08 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1716 - Calculate Money in Leetcode Bank  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/calculate-money-in-leetcode-bank/> 

![](/assets/img/sample/leetcode/1716/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1716/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n이 들어옵니다. 1원부터 시작해서 일주일간 1원씩 돈을 냅니다.
- 일주일이 지나면 돈은 초기화되면 전 주의 1원을 더한 값부터 위의 방식을 반복합니다.
- 위 두 과정을 반복하였을 때 지불한 금액을 리턴하세요.


-----  

## 5. code  

코드설명

- 각 리스트의 요소는 사각형의 폭과 높이입니다.
- 변을 잘라서 정사각형을 만들 때 가장 폭이 긴 정사각형의 갯수를 리턴하세요.



**python**

```python
class Solution:
    def totalMoney(self, n: int) -> int:
        mod = n // 7
        res = n % 7
        ans = 0
        for i in range(mod):
            ans += 28 + i * 7
        for i in range(1,res+1) : 
            ans += i + mod
        return ans               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1716/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


