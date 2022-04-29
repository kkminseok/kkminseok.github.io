---
title: leetcode(리트코드)-1694 Reformat Phone Number(PYTHON)
author: 강민석
date: 2021-08-10 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1694 - Reformat Phone Number  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reformat-phone-number/> 

![](/assets/img/sample/leetcode/1694/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1694/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- number가 주어집니다.
- number의 요소가 숫자인 것만 뽑고 3개씩 '-'로 구분합니다. 만약 남은 숫자요소의 갯수가 4개라면 2개-2개로 분리합니다.
- 분리된 문자열을 리턴하세요.



-----  

## 5. code  

코드설명



**python**

```python
class Solution:
    def reformatNumber(self, number: str) -> str:
        #brute list
        sz = len(number)
        resli = deque()
        count = 0 
        for i in range(sz):
            if number[i].isdigit():
                count+=1
                resli.append(number[i])
                if count%3 == 0 :
                    resli.append("-")
        if count%3 == 0 :
            resli.pop()
        if count%3 == 1 : 
            resli[-3],resli[-2] = resli[-2], resli[-3]
        return ''.join(resli)
                   
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1694/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


