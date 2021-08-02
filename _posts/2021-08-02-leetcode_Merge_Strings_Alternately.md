---
title: leetcode(리트코드)-1768 Merge Strings Alternately(PYTHON)
author: 강민석
date: 2021-08-02 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1768 - Merge Strings Alternately  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/merge-strings-alternately/> 

![](/assets/img/sample/leetcode/1768/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1768/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- word1과 word2가 들어옵니다. 
- 두 문자열을 지그재그식으로 확인하여 문자열을 만들 때 그 문자열을 리턴하세요.





-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        size1 = len(word1)
        size2 = len(word2)
        i = 0
        j = 0
        res = ""
        while i + j < size1 + size2:
            if i < size1 : 
                res += word1[i]
                i+=1
            if j < size2 : 
                res += word2[j]
                j+=1
                
        return res              
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1768/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


