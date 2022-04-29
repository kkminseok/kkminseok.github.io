---
title: leetcode(리트코드)344-Reverse String
author: 강민석
date: 2021-04-14 11:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 344 - Reverse String 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reverse-string/>  

![](/assets/img/sample/leetcode/344/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/344/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 문자 벡터가 있습니다. 뒤집으면 됩니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        reverse(s.begin(),s.end());
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 72% python ??%** 
![](/assets/img/sample/leetcode/344/result.JPG)  






