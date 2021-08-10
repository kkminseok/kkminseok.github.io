---
title: leetcode(리트코드)-1576 Replace All'?s to Avoid Consecutive Repeating Characters(PYTHON)
author: 강민석
date: 2021-08-10 04:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1576 - Replace All'?s to Avoid Consecutive Repeating Characters  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/replace-all-s-to-avoid-consecutive-repeating-characters/> 

![](/assets/img/sample/leetcode/1576/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1576/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열에 대해 "?"에 임의의 문자를 넣습니다.
- 단 왼쪽 문자와 오른쪽 문자와 같으면은 안됩니다.




-----  

## 5. code  

코드설명

- 랜덤함수 사용.


**python**

```python
import random
class Solution:
    def modifyString(self, s: str) -> str:
        res = ""
        left = 0 
        right = 0
        for i in range(len(s)):
            if s[i] == "?" : 
                if i!= 0 : 
                    left = ord(res[i-1])
                if i!= len(s)-1 :
                    right = ord(s[i+1])
                rand = random.randrange(97,122)
                while rand == left or rand == right :
                    rand = random.randrange(97,122)
                res += chr(rand)
            else : res += s[i]
        return res
                
                    
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1576/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


