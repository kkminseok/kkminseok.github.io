---
title: leetcode(리트코드)5월15일 challenge65-Valid Number
date: 2021-05-15 10:01:56 +0800
categories: [leetcode,Hard]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 15일 - Valid Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/valid-number/>  

![](/assets/img/sample/leetcode/65/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/65/input.JPG)  


-----  

## 3. 분류 및 난이도

Hard 난이도입니다.  
5월 15일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 들어온 string이 숫자로 표현될 수 있는 지 확인합니다.
- 모든 경우의 수를 따져야해서 bad가 많은 문제 입니다.(창의성이 없어서 그런 듯.)





-----  

## 5. code


**python**

```python
class Solution:
    def isNumber(self, s: str) -> bool:
        s = s.strip()
        numberS = False
        eS = False
        pointS = False
        numberAE = True
        
        for i in range(len(s)):
            if s[i]>="0" and s[i] <="9" : 
                numberS = True
                numberAE = True
            elif s[i] == ".":
                if eS or pointS :
                    return False
                pointS = True
            elif s[i] == "e" or s[i] == "E":
                if eS or not numberS : 
                    return False
                numberAE = False
                eS = True
            elif s[i] =="-" or s[i] =="+" :
                if i != 0 and s[i-1] !="e" : 
                    return False
            else:
                return False
        
        return numberS and numberAE
```



-----

## 6. 결과 및 후기, 개선점

나중에 다시 풀 것.




