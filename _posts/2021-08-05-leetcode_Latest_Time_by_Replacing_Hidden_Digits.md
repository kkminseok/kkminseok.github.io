---
title: leetcode(리트코드)-1736 Latest Time by Replacing Hidden Digits(PYTHON)
author: 강민석
date: 2021-08-05 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1736 - Latest Time by Replacing Hidden Digits  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/latest-time-by-replacing-hidden-digits/> 

![](/assets/img/sample/leetcode/1736/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1736/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- time이 주어집니다
- "?"로 된 곳에 임의의 숫자를 넣을 때 가장 오래된 시간을 만들어서 리턴하세요 (00:00 ~ 23:59)

-----  

## 5. code  

코드설명

- 하드코딩


**python**

```python
class Solution:
    def maximumTime(self, time: str) -> str:
        res = ""
        for i in range(len(time)):
            if time[i] == "?":
                if i == 0 :
                    if time[1] == "?" or time[1] <"4":
                        res +="2"
                    else : 
                        res +="1"
                elif i == 1 :
                    if res[0] == "2":
                        res += "3"
                    else :
                        res += "9"
                elif i == 3 :
                    res += "5"
                elif i == 4 :
                    res += "9"
                     
            else:
                res += time[i]
        return res
            
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1736/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


