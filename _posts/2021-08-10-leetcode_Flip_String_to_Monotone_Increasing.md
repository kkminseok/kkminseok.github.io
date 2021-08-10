---
title: leetcode(리트코드)-926 Flip String to Monotone Increasing(PYTHON)
author: 강민석
date: 2021-08-10 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 926 - Flip String to Monotone Increasing  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/flip-string-to-monotone-increasing/> 

![](/assets/img/sample/leetcode/926/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/926/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Monotone이란 내리막이 없는 수를 말합니다.
- "00000" 이나 "11111", "0001111"와 같은 수를 말합니다.
- 수를 0이나 1로 뒤집을 수 있을 때 최소한으로 뒤집어서 Monotone를 만들어야합니다.
- 그 수를 리턴하세요.



-----  

## 5. code  

코드설명

- brute하게 풀면 시간초과가 나옵니다.
- "01"이 되는 구간을 찾아서 점화식을 구해서 풀었습니다.
- 점화식의 내용은 "01"구간에서 왼쪽구간에서 1의 개수
- 오른쪽 구간에서 남은 요소의 숫자 - (총 1의개수에서 + 왼쪽에 나온 1)의 갯수를 뺍니다.
- 왼쪽구간에서 구한 수와 오른쪽에서 구한 수를 더하면 됩니다.

**python**

```python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
        #re
        sz = len(s)
        totalone = s.count('1')
        totalzero = s.count('0')
        if totalone == sz or totalzero == sz : 
            return 0
        zero = 0 
        one = 0
        res = 987564321
        for i in range(sz-1) :
            if s[i] =='0' and s[i+1] == '1' : 
                res = min(res, sz - i - 1 - totalone + 2 * one)
            if s[i] == '0':
                zero += 1
            else : 
                one += 1
        return min(res, sz - s.count('1'), sz - s.count('0'))
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/926/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


