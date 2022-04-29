---
title: leetcode(리트코드)-67 Add Binary(python)
author: 강민석
date: 2021-05-21 04:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 67 - Add Binary 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/add-binary/> 

![](/assets/img/sample/leetcode/67/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/67/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- 문자열로 들어온 숫자를 binary로 표현되었다고 생각하고 두 숫자를 더한 binary string을 리턴합니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        i = len(a)-1
        j = len(b)-1
        carry = 0
        num = 0
        res = ""
        while i >=0 and j >=0 :
            sum = int(a[i]) + int(b[j]) + carry
            carry = int(sum) //2
            num = int(sum)%2
            res +=str(num)
            i-=1
            j-=1
        while i>=0 : 
            sum = int(a[i]) + carry
            carry = int(sum) //2
            num = int(sum)%2
            res +=str(num)
            i-=1
        while j>=0:
            sum = int(b[j]) + carry
            carry = int(sum) //2
            num = int(sum)%2
            res +=str(num)
            j-=1
        if carry ==1:
            res+="1"
        return res[::-1]
```



-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/67/result.JPG)  

필요시 c++로 짜드리겠습니다.



