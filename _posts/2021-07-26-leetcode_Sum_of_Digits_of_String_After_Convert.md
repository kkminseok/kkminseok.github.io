---
title: leetcode(리트코드)-1945 Sum of Digits of String After Convert(PYTHON)
author: 강민석
date: 2021-07-26 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1945 - Sum of Digits of String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sum-of-digits-of-string-after-convert/> 

![](/assets/img/sample/leetcode/1945/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1945/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s와 k가 들어옵니다.
- s는 문자열이고 해당 문자들을 알파벳 순서대로 반환하여 각자리에 나타냅니다.
- 나타낸 수를 k만큼 반복하여 자리마다 수를 더해 결과값을 리턴합니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def getLucky(self, s: str, k: int) -> int:
        res = 0
        convert = ""
        loop = 0
        while loop <= k :
            if loop == 0 :
                for i in range(len(s)):
                    convert += str((ord(s[i]) - 96))
            else : 
                res = 0 
                for i in range(len(s)):
                    res += int(s[i])
                convert = str(res)
            loop+=1
            s = convert
            convert = ""
        return res
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1945/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


