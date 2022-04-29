---
title: leetcode(리트코드)-1807 Evaluate the Bracket Pairs of a String(PYTHON)
author: 강민석
date: 2021-08-10 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1807 - Evaluate the Bracket Pairs of a String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/evaluate-the-bracket-pairs-of-a-string/> 

![](/assets/img/sample/leetcode/1807/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1807/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- ()안에 들어있는 것을 바꿔야합니다.
- knowledge들어있는 [0]번째 요소는 키값 [1]번째 요소는 value로 ()안에 있는 값과 일치하면 value값으로 바꿉니다.
- 만약 일치하는 key값이 없다면 "?"를 넣어줍니다.



-----  

## 5. code  

코드설명




**python**

```python
class Solution:
    def evaluate(self, s: str, knowledge: List[List[str]]) -> str:
        #list to dic
        dic = {}
        for i in range(len(knowledge)):
            dic[knowledge[i][0]] = knowledge[i][1]
        #임시로 문자열을 담을 변수
        temp = ""
        #결과 문자열
        res = ""
        i = 0
        sz = len(s)
        while i < sz:
            if s[i] =="(" : 
                i+=1
                while i< sz and s[i] != ")":
                    temp += s[i]
                    i+=1
                if temp in dic : 
                    res+= dic[temp]
                else : 
                    res += "?"
            else : 
                res += s[i]
            temp = ""
            i+=1
        return res          
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1352/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


