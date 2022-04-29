---
title: leetcode(리트코드)-1189 Maximum Number of Ballons(PYTHON)
author: 강민석
date: 2021-09-14 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1189 - Maximum Number of Ballons문제입니다.**

## 1. 문제

<https://leetcode.com/problems/maximum-number-of-balloons/>

!["None"](/assets/img/sample/leetcode/1189/Problem.JPG)

-----  

## 2. Input , Output

!["None"](/assets/img/sample/leetcode/1189/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 문자열이 들어옵니다.
- 들어온 문자열을 재배열할 때 "ballon"이라는 글자를 몇개 만들 수 있는 지 그 갯수를 리턴하세요

-----  

## 5. code  

### python

```python
class Solution:
    def maxNumberOfBalloons(self, text: str) -> int:
        res = 987654321
        res = min(text.count('b'),res)
        res = min(text.count('a'),res)
        res = min(text.count('l')//2,res)
        res = min(text.count('n'),res)
        return min(res,text.count('o')//2)   
```

-----

## 6. 결과 및 후기, 개선점

!["None"](/assets/img/sample/leetcode/1189/result.JPG)  

필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.
