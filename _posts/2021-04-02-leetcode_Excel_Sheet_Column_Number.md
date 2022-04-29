---
title: leetcode(리트코드)171-Excel Sheet Column Number
author: 강민석
date: 2021-04-02 00:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 171 - Excel Sheet Column Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/excel-sheet-column-number/>  

![](/assets/img/sample/leetcode/171/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/171/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- A = 1, B = 2, ... , Z = 26 값을 가집니다. 문자열이 들어왔을 때 엑셀처럼 값을 반환해야합니다. AA는 27 AB는 28....

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int titleToNumber(string columnTitle) {
        int result = 0 ;
        int pownum = 0;
        for(int i = columnTitle.size()-1;i>=0;--i){
            result+= (pow(26,pownum++) * (columnTitle[i]-64) );
        }
        return result;
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/171/result.JPG)  






