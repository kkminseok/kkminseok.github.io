---
title: leetcode(리트코드)172-Factorial Trailing Zeroes
author: 강민석
date: 2021-04-05 07:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 172 - Factorial Trailing Zeroes 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/factorial-trailing-zeroes/>  

![](/assets/img/sample/leetcode/172/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/172/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- int 형 정수로 들어온 값의 팩토리얼을 구해서 0의 개수를 리턴합니다.
- 이건 그냥 공식이 따로 있을 것 같아서 discuss를 봤습니다. 때문인지 평점이 좋지는 않습니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int trailingZeroes(int n) {
        long long i = 5;
        int result = 0;
        for(i; n/i>0; i*=5)
            result += n/i;
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/172/result.JPG)  






