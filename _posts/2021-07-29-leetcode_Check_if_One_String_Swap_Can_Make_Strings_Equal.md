---
title: leetcode(리트코드)-1790 Check if One String Swap Can Make Strings Equal(PYTHON)
author: 강민석
date: 2021-07-29 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1790 - Check if One String Swap Can Make Strings Equal  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-one-string-swap-can-make-strings-equal/> 

![](/assets/img/sample/leetcode/1790/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1790/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- s1과 s2가 주어집니다.
- 둘의 문자가 다른 경우 하나의 알파벳을 스왑하여 같게 만들 수 있습니다.
- 만약 두 개 이상의 알파벳을 스왑하여 같게만들어야 하는경우는 False를 리턴하세요. 또는 같게 만들게 할 수 없는 경우 False를 리턴하세요.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def areAlmostEqual(self, s1: str, s2: str) -> bool:
        if len(s1) != len(s2):
            return False
        check = 0
        for i in range(len(s1)):
            if s1[i] != s2[i] :
                if check >=2 :
                    return False
                else : 
                    if s2.find(s1[i]) != -1:
                        check+=1
                    else : 
                        return False
        return True if check %2 == 0 else False
                
            
        
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1790/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


