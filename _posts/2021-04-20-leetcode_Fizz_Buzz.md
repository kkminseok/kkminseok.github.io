---
title: leetcode(리트코드)412-Fizz Buzz
author: 강민석
date: 2021-04-20 11:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 412 - Fizz Buzz 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/fizz-buzz/>  

![](/assets/img/sample/leetcode/412/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/412/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

인덱스가  
- 5와 3으로 나눠지면 FizzBuzz
- 3으로 나눠지면 Fizz
- 5로 나눠지면 Buzz
- 나머지는 인덱스 숫자를 넣어 리턴합니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> res;
        for(int i = 1 ; i <= n ;++i){
            if(i%15 ==0) res.push_back("FizzBuzz");
            else if(i%3==0) res.push_back("Fizz");
            else if(i%5==0) res.push_back("Buzz");
            else res.push_back(to_string(i));
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 75% python ??%** 
![](/assets/img/sample/leetcode/412/result.JPG)  






