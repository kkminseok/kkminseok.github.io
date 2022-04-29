---
title: leetcode(리트코드)-1832 Check if the Sentence Is Pangram(python)
author: 강민석
date: 2021-07-13 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1832 - Check if the Sentence Is Pangram  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-the-sentence-is-pangram/> 

![](/assets/img/sample/leetcode/1832/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1832/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- pangram이란, sentence로 주어진 문자열에 모든 소문자 영어 알파벳이 한 번 이상 나오면 True이고 아니면 False인 문자열을 말합니다.
- pangram를 판단하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def checkIfPangram(self, sentence: str) -> bool:
        dic = {}
        for word in sentence:
            if word not in dic :
                 dic[word] = 1
                 if len(dic) == 26 :
                    return True
        return False
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1832/result.JPG)  

필요시 c++로 짜드리겠습니다.



