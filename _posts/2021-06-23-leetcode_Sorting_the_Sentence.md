---
title: leetcode(리트코드)-1859 Sorting the Sentence(python)
author: 강민석
date: 2021-06-23 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1859 - Sorting the Sentence 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sorting-the-sentence/> 

![](/assets/img/sample/leetcode/1859/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1859/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- input으로 문자열이 들어옵니다. 
- 띄어쓰기를 기준으로 각 문자열의 끝에 번호가 주어집니다.
- 해당 번호 순서대로 문자열을 재배치하여 리턴합니다.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def sortSentence(self, s: str) -> str:
        stsp = s.split(" ")
        resli = [""] * len(stsp)
        res = ""
        for i in range(len(stsp)):    
            resli[int(stsp[i][-1])-1] = stsp[i][:-1]
        for i in range(len(resli)):
            res += resli[i] + " "
        return res[:-1]
                
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1859/result.JPG)  

필요시 c++로 짜드리겠습니다.



