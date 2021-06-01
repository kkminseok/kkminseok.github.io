---
title: leetcode(리트코드)-1816 Truncate Sentence(python)
author: 강민석
date: 2021-06-01 06:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1816 - Truncate Sentence 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/truncate-sentence/> 

![](/assets/img/sample/leetcode/1816/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1816/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- k로 들어온 수만큼 어절을 리턴합니다.



-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def truncateSentence(self, s: str, k: int) -> str:
        splitlist = s.split()
        res=""
        for i in range(k):
            res+=splitlist[i]+" "
        return res[:-1]
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1816/result.JPG)  

필요시 c++로 짜드리겠습니다.



