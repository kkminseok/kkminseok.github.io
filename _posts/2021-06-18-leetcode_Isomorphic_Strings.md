---
title: leetcode(리트코드)-205 Isomorphic Strings(python)
author: 강민석
date: 2021-06-18 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 205 - Isomorphic Strings 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/isomorphic-strings/> 

![](/assets/img/sample/leetcode/205/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/205/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s의 패턴과 t의 패턴이 같으면 True를 리턴 다르면 False를 리턴합니다.
- 각 문자는 1:1로 매칭되어야하므로 dic자료구조를 이용해서 저장시켜줍니다.
- 만약 이미 저장되었으면 하나의 리스트에서 검사를 해서 False를 리턴해줄 수 있도록 합니다.



-----  

## 5. code  

코드설명

**python**

```python

class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        dic = {}
        #150은 아스키코드 갯수. 이미 들어와있는데 dic에 추가하면 False를 리턴
        check = [False] * 150
        for i in range(len(s)):
            if s[i] not in dic :
                if check[ord(t[i])] == True:
                    return False
                dic[s[i]] = t[i]
                check[ord(t[i])] = True
            else : 
                if dic[s[i]] != t[i]:
                    return False
        return True
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/205/result.JPG)  

필요시 c++로 짜드리겠습니다.



