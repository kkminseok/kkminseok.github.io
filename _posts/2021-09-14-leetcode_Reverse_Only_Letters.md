---
title: leetcode(리트코드)-917 Reverse Only Letters(PYTHON)
author: 강민석
date: 2021-09-14 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 917 - Reverse Only Letters문제입니다.**

## 1. 문제

<https://leetcode.com/problems/reverse-only-letters/>

!["None"](/assets/img/sample/leetcode/917/Problem.JPG)

-----  

## 2. Input , Output

!["None"](/assets/img/sample/leetcode/917/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 문자열이 들어옵니다.
- 문자열을 반대로 출력할 것인데, 원본문자열의 인덱스에서 문자가 아닌 것은 자리를 그대로 유지하여 출력해야합니다.

-----  

## 5. code  

### python

```python
class Solution:
    def reverseOnlyLetters(self, s: str) -> str:
        res = ""
        def findalpha(reverseidx) : 
            while reverseidx > 0 and not s[reverseidx].isalpha() : 
                reverseidx-=1
            return reverseidx
        reverseidx = findalpha(len(s)-1)
        for char in s : 
            if char.isalpha() : 
                res += s[reverseidx]
                reverseidx = findalpha(reverseidx-1)
            else : 
                res += char
        return res
                
```

-----

## 6. 결과 및 후기, 개선점

!["None"](/assets/img/sample/leetcode/917/result.JPG)  

필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.
