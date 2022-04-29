---
title: leetcode(리트코드)-1759 Count Number of Homogenous Substrings(PYTHON)
author: 강민석
date: 2021-08-13 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1759 - Count Number of Homogenous Substrings  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-number-of-homogenous-substrings/> 

![](/assets/img/sample/leetcode/1759/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1759/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s라는 문자열이 주어집니다.
- 예시처럼 반복되는 경우의 수를 모두 구해 더한 값을 리턴해야합니다.






-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def countHomogenous(self, s: str) -> int:
        res = 0 
        idx = 0 
        mod = 10**9 + 7 
        while idx < len(s)-1:
            temp = 1
            while idx < len(s)-1 and s[idx] == s[idx+1] :
                temp += 1
                idx += 1
            res += temp * (temp + 1) // 2
            idx+=1
            
        if idx != len(s) : 
            res+=1
        return res % mod
                    
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1759/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


