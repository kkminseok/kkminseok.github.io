---
title: leetcode(리트코드)-1544 Make The String Great(PYTHON)
author: 강민석
date: 2021-08-09 05:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1544 - Make The String Great  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/make-the-string-great/> 

![](/assets/img/sample/leetcode/1544/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1544/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s가 주어집니다. 임의의 인덱스들에 대해 그 전의 인덱스값이 고른 인덱스의 소문자의 형태거나 그 반대(소문자인데 대문자)인 경우 요소를 삭제하여 남은 문자열을 리턴합니다.



-----  

## 5. code  

코드설명

- queue를 사용하였습니다.


**python**

```python
class Solution:
    def makeGood(self, s: str) -> str:
        dq = deque()
        for i in range(len(s)):
            if not dq : 
                dq.append(s[i])
                continue
            if dq[-1].isupper() and ord(dq[-1]) + 32 == ord(s[i]) : 
                dq.pop()
            elif dq[-1].islower() and ord(dq[-1]) - 32  == ord(s[i]):
                dq.pop()
            else : 
                dq.append(s[i])
        return ''.join(dq)
                         
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1544/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


