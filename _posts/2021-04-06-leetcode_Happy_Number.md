---
title: leetcode(리트코드)202-Happy Number
author: 강민석
date: 2021-04-06 10:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 202 - Happy Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/happy-number/>  

![](/assets/img/sample/leetcode/202/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/202/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 1이될 때까지 자릿수에 제곱한 수를 더해 봅니다. 만약 1이 될 수 없다면 false를 리턴합니다.
- 평점이 좋지는 않은데 종료조건이 너무 명확하지 않아서 그런 것 같습니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    long long cal(long long happy){
        long long res=0;
        while(happy!=0){
            res += pow(happy%10,2);
            happy/=10;
        }
        return res;
    }
    
    bool isHappy(int n) {
        long long happy = n;
        long long fasthappy = n;
        do{
            happy = cal(happy);
            fasthappy = cal(fasthappy);
            fasthappy = cal(fasthappy);
        }while(happy != fasthappy);
        return happy == 1 ? true : false;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/202/result.JPG)  






