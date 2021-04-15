---
title: leetcode(리트코드)4월15일 challenge509-Fibonacii Number
author: 강민석
date: 2021-04-15 10:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 15일 - Fibonacii Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/fibonacci-number/>  

![](/assets/img/sample/leetcode/509/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/509/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
4월 15일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 피보나치 수열





-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int DP[31] = {};
    int fib(int n) {
        if(n==0) return 0;
        if(n==1) return 1;
        if(DP[n]!=0) return DP[n];
        return DP[n] = fib(n-1) + fib(n-2);
    }
};
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/509/result.JPG)  




