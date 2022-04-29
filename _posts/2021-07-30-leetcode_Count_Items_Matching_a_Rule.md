---
title: leetcode(리트코드)-1773 Count Items Matching a Rule(PYTHON)
author: 강민석
date: 2021-07-30 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1773 - Count Items Matching a Rule  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-items-matching-a-rule/> 

![](/assets/img/sample/leetcode/1773/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1773/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- ruleKey와 ruleValue가 주어지는데 items에서 일치하는 값들을 찾아 그 갯수를 리턴합니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def countMatches(self, items: List[List[str]], ruleKey: str, ruleValue: str) -> int:
        res = 0 
        idx = -1
        if ruleKey == "type" : 
            idx = 0
        elif ruleKey == "color":
            idx = 1
        else : 
            idx = 2
        for i in range(len(items)):
            if items[i][idx] == ruleValue : 
                res+=1
        return res
              
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1773/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


