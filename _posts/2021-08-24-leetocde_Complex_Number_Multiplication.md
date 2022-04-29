---
title: leetcode(리트코드)-537 Complex Number Multiplication(PYTHON)
author: 강민석
date: 2021-08-24 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 537 - Complex Number Multiplication  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/complex-number-multiplication/> 

![](/assets/img/sample/leetcode/537/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/537/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- str로 복소수가 2개 들어옵니다.
- 두 str로 이루어진 복소수를 곱한 결과를 str자료형으로 리턴하세요.





-----  

## 5. code  

코드설명



**python**

```python
class Solution:
    def complexNumberMultiply(self, num1: str, num2: str) -> str:
        idx1 = num1.index('+')
        idx2 = num2.index('+')
        real1 = num1[:idx1]
        real2 = num2[:idx2]
        imag1 = num1[idx1+1:num1.find('i')]
        imag2 = num2[idx2+1:num2.find('i')]
        num1 = complex(int(real1),int(imag1))
        num2 = complex(int(real2),int(imag2))
        res = num1 * num2
        temp = ""
        if res.real == 0 :
            temp += "0+"
        else : 
            temp += str(res.real)[:-2] + "+"
        temp += str(res.imag)[:-2] + "i"
        return temp
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/537/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


