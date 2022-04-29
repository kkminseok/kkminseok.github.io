---
title: leetcode(리트코드)-1784 Check if Binary String Has Most One Segment of Ones(PYTHON)
author: 강민석
date: 2021-07-30 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1784 - Check if Binary String Has Most One Segment of Ones  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-binary-string-has-at-most-one-segment-of-ones/> 

![](/assets/img/sample/leetcode/1784/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1784/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 1이 다 나온 뒤에 0 이나오는건 상관이 없지만 1이 다 나오지 않고 0이 나오면 False를 리턴합니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def checkOnesSegment(self, s: str) -> bool:
        if len(s) == 1 :
            return True
        for i in range(len(s)-1,0,-1):
            if s[i] == "1" and s[i-1] == "0" :
                return False
        return True   
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1784/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


