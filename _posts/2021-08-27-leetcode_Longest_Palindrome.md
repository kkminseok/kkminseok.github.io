---
title: leetcode(리트코드)-409 Longest Palindrome(PYTHON)
author: 강민석
date: 2021-08-27 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 409 - Longest Palindrome  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-palindrome/> 

![](/assets/img/sample/leetcode/409/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/409/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열s가 들어옵니다. 문자를 재조합할 때 가장 긴 Palindrome을 찾아 그 길이를 리턴합니다.





-----  

## 5. code  



**python**

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        dic = {}
        for i in range(len(s)):
            if s[i] not in dic :
                dic[s[i]] = 1
            else : 
                dic[s[i]] += 1
        check = False
        li = []
        for counting in dic :
            if dic[counting] %2 == 1 :
                check = True
            li.append(dic[counting])
        res = 0
        for i in range(len(li)) : 
            res += li[i] - (li[i] % 2)
        return res if check == False else res+1 
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/409/result.JPG)  



필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


