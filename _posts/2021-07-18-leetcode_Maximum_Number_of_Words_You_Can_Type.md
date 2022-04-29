---
title: leetcode(리트코드)-1935 Maximum Number of Words You Can Type(PYTHON)
author: 강민석
date: 2021-07-18 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1935 - Maximum Number of Words You Can Type  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-number-of-words-you-can-type/> 

![](/assets/img/sample/leetcode/1935/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1935/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- text와 brokenLetters가 주어집니다.
- brokenLetters은 키보드가 박살난 것들입니다.
- text를 공백을 기준으로 나누었을 때 몇개의 텍스트를 완벽히 타이핑이 가능한 지 리턴합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def canBeTypedWords(self, text: str, brokenLetters: str) -> int:
        text = text.split()
        chrlist = [0] * 26
        for i in range(len(brokenLetters)) :
            chrlist[ord(brokenLetters[i])-97] = 1
        res = 0
        for word in text:
            check = False
            for j in range(len(word)):
                if chrlist[ord(word[j])-97] == 1:
                    check = True
                    break
            if not check: 
                res+=1
                
        return res
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1935/result.JPG)  





