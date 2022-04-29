---
title: leetcode(리트코드)-1910 Remove All Occurrences of a Substring(PYTHON)
author: 강민석
date: 2021-07-17 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1910 - Remove All Occurrences of a Substring  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-all-occurrences-of-a-substring/> 

![](/assets/img/sample/leetcode/1910/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1910/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- input과 part가 주어집니다.
- part를 input에서 찾아 모든 부분 문자열을 지워버린 문자열을 리턴합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def removeOccurrences(self, s: str, part: str) -> str:
        size = len(part)
        while True : 
            idx = s.find(part)
            if idx == -1 : 
                break
            else : 
                s = s[:idx] + s[idx + size:]
        return s
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1910/result.JPG)  





