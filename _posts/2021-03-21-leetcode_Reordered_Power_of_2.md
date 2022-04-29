---
title: leetcode(리트코드)3월21일 challenge869-Reordered Power of 2
author: 강민석
date: 2021-03-21 12:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 20일 - Reordered Power of 2 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reordered-power-of-2/>  

![](/assets/img/sample/leetcode/869/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/869/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 20일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 정수 N이 들어옵니다. 정수 N을 2진법으로 나타내어 재배열 했을 때 그 수가 2의 거듭제곱 수로 나타낼 수 있으면 true 나타낼 수 없으면 false를 리턴합니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    bool reorderedPowerOf2(int N) {
        long long result = change(N);
        for(int i = 0;i<=31;++i)
        {
            if(result == change(1<<i)) return true;
        }
        return false;
        
    }
    long long change(int n){
        //2진수로 바꿔버림.
        long long result = 0;
        for(;n;n/=10) result += pow(10,n%10);
        return result;
    }
};


```

-----

## 6. 결과 및 후기, 개선점

**c++ 99%**
![](/assets/img/sample/leetcode/869/result.JPG)  

