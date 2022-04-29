---
title: leetcode(리트코드)-434 Number of Segments in a StringNumber of Segments in a String(PYTHON)
author: 강민석
date: 2021-08-28 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 434 - Number of Segments in a StringNumber of Segments in a String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-segments-in-a-string/> 

![](/assets/img/sample/leetcode/434/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/434/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열이 들어옵니다. 어절의 개수를 리턴하세요.





-----  

## 5. code  



**python**

```python
class Solution:
    def countSegments(self, s: str) -> int:
        if len(s) == 0  or s.count(' ') == len(s) :
             return 0
        res = 0 
        idx = 0 
        while idx < len(s) : 
            if s[idx] != ' ': 
                res += 1
                while idx < len(s) and s[idx] != ' ':
                    idx+=1
            else: 
                idx += 1
        return res
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/434/result.JPG)  



필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


