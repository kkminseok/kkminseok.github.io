---
title: leetcode(리트코드)-1805 Number of Different Integers in a String(PYTHON)
author: 강민석
date: 2021-07-19 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1805 - Number of Different Integers in a String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-different-integers-in-a-string/> 

![](/assets/img/sample/leetcode/1805/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1805/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- word가 주어집니다. 
- 숫자만 뽑아낼 때 중복없이 뽑아내야합니다.
- 뽑아낸 숫자의 갯수를 리턴하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def numDifferentIntegers(self, word: str) -> int:
        s = set()
        i = 0 
        while i < len(word) :
            temp = "" 
            while i <len(word) and word[i].isdigit() :
                temp += word[i]
                i+=1
            if temp != "" : 
                temp = int(temp)
                s.add(temp)
                temp = ""
                i-=1
            i+=1
        return len(s)
        
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1805/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


