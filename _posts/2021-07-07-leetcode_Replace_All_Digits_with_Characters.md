---
title: leetcode(리트코드)-1844 Replace All Digits with Characters(python)
author: 강민석
date: 2021-07-07 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1844 - Replace All Digits with Characters  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/replace-all-digits-with-characters/> 

![](/assets/img/sample/leetcode/1844/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1844/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- input으로 string이 주어집니다. 숫자의 의미는 전에 나온 문자를 그 숫자만큼 더하란 것입니다. 즉 a에 1을 더한 것은 b입니다.
-완성된 문자열을 리턴하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def replaceDigits(self, s: str) -> str:
        res = ""
        for i in range(len(s)):
            if i%2 == 0 :
                temp = ord(s[i])
                res += s[i]
            else : 
                res+= chr(temp+ int(s[i]))
        return res
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1844/result.JPG)  

필요시 c++로 짜드리겠습니다.



