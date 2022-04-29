---
title: leetcode(리트코드)-1869 Longer Contiguous Segments of Ones than Zeros(python)
author: 강민석
date: 2021-06-21 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1869 - Longer Contiguous Segments of Ones than Zeros 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longer-contiguous-segments-of-ones-than-zeros/> 

![](/assets/img/sample/leetcode/1869/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1869/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열이 들어옵니다. 연속되는 1과 0을 찾아서 그 길이를 얻었을 때 연속되는 1의 최대의 길이가 연속되는 0의 최대의 길이보다 크면 True 아니라면 False를 리턴하세요.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def checkZeroOnes(self, s: str) -> bool:
        zero = 0
        one = 0
        count = 0 
        i = 0
        while i < len(s):
            while i < len(s) and s[i] == "0"  : 
                i+=1
                count+=1
            zero = max(zero,count)
            count= 0
            while i<len(s) and s[i] == "1" :
                i+=1
                count+=1
            one = max(one,count)
            count= 0 
        return one>zero      
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1869/result.JPG)  

필요시 c++로 짜드리겠습니다.



