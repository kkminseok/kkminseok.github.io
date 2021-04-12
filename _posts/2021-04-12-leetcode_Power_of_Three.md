---
title: leetcode(리트코드)326-Power of Three
author: 강민석
date: 2021-04-12 11:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 326 - Power of Three 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/power-of-three/>  

![](/assets/img/sample/leetcode/326/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/326/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 들어온 n값이 3의 거듭제곱으로 나타낼 수 있으면 true 없으면 false를 리턴합니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        if(n<=0) return false;
        while(n!=1){
            if(n%3 !=0) return false;
            n/=3;
        }
        return true;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 86% python ??%** 
![](/assets/img/sample/leetcode/326/result.JPG)  






