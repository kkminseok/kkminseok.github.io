---
title: leetcode(리트코드)-1880 Check if Word Equals Summation of Two Words(python)
author: 강민석
date: 2021-06-20 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1880 - Check if Word Equals Summation of Two Words 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-word-equals-summation-of-two-words/> 

![](/assets/img/sample/leetcode/1880/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1880/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 숫자가 들어있는 정사각형이 주어집니다. 정사각형을 90도씩 돌렸을 때 target과 일치하면 True 아니면 False를 리턴합니다.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def isSumEqual(self, firstWord: str, secondWord: str, targetWord: str) -> bool:
        firstnum = ""
        secondnum =""
        targetnum = ""
        for i in range(len(firstWord)):
            firstnum += str(ord(firstWord[i])-97)
        for i in range(len(secondWord)):
            secondnum += str(ord(secondWord[i]) - 97)
        for i in range(len(targetWord)) : 
            targetnum += str(ord(targetWord[i]) - 97)
        return int(targetnum) == (int(firstnum) + int(secondnum))
            
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1880/result.JPG)  

필요시 c++로 짜드리겠습니다.



