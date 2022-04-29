---
title: leetcode(리트코드)-935 Knight Dialer(PYTHON)
author: 강민석
date: 2021-07-26 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 935 - Knight Dialer  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/knight-dialer/> 

![](/assets/img/sample/leetcode/935/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/935/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 체스의 knight라고 생각하고 전화번호를 누른다고 가정합니다.
- " 1 2 3 "
- " 4 5 6 "
- " 7 8 9 "
- " * 0 # "
- 이라고 할 때 숫자만 눌러야합니다. n자리수의 전화번호를 만들 때 가능한 경우의 수를 10^9+7로 나누어 리턴합니다.



-----  

## 5. code  

코드설명
- 일단 5번을 누르는 경우는 n이 1인 경우말고는 없습니다.
- 마지막으로 누른 번호가 4와 6번인 경우에는 각 3가지의 경우가 있습니다.
- 마지막으로 누른 번호가 4와 6번이 아닌 경우는 각 2가지의 경우가 있습니다.


**python**

```python
class Solution:
    def knightDialer(self, n: int) -> int:
        x0 = x1 = x2 = x3 = x4 = x5 = x6 = x7 = x8 = x9 = 1
        for i in range(n-1) : 
            x0,x1,x2,x3,x4,x6,x7,x8,x9 = \
            x4 + x6, x6 + x8, x7 + x9,\
            x4 + x8, x3 + x0 + x9, x0 + x1 + x7,\
            x2 + x6, x1 + x3, x2 + x4
            
            
        res = x0 + x1 + x2 + x3 + x4 + x5 + x6 + x7 + x8 + x9
        if n != 1: 
            res-=1
        return (res) % (10**9 + 7)
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/935/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


