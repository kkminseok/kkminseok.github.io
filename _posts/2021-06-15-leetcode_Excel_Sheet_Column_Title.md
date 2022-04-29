---
title: leetcode(리트코드)-168 Excel Sheet Column Title(python)
author: 강민석
date: 2021-06-15 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 168 - Excel Sheet Column Title 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/excel-sheet-column-title/> 

![](/assets/img/sample/leetcode/168/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/168/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 숫자가 들어오면 Excel에서의 행과 열의 값을 구해 리턴하세요.




-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        dic = {}
        res = ""
        for i in range(26):
            #숫자를 아스키코드로 변환
            asc = chr(i+65)
            dic[i+1] = asc
        while columnNumber : 
            temp = columnNumber%26 
            if temp == 0 :
                res+="Z"
                if columnNumber == 26 : 
                    break
            else:
                res += dic[temp]
            #나누어 떨어지는 경우를 고려
            columnNumber = (columnNumber-1)//26
        return res[::-1]
        
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/168/result.JPG)  

필요시 c++로 짜드리겠습니다.



