---
title: leetcode(리트코드)4월06일 challenge1551-Minimum Operations to Make Array Equal
author: 강민석
date: 2021-04-06 13:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 06일 - Minimum Operations to Make Array Equal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-operations-to-make-array-equal/>  

![](/assets/img/sample/leetcode/1551/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1551/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 06일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 배열의 요소는 [인덱스 * 2 + 1]입니다. 
- 한 가지 요소를 선택하여 1을 올리고 한 가지 요소를 선택하여 1을 내릴 수 있습니다.
- 최소한으로 올리고 내려서 모든 배열의 수를 맞출 때 올리고 내린 횟수를 리턴해야합니다.





-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int minOperations(int n) {
        int* DP = new int[n+1];
        DP[0] =0;
        DP[1] = 0;
        for(int i =2;i<=n;++i)
            DP[i] = DP[i-2] + i-1;
        return DP[n];
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 100%**
![](/assets/img/sample/leetcode/1551/result.JPG)  


