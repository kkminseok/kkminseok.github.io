---
title: leetcode(리트코드)66-Plus One
author: 강민석
date: 2021-03-25 02:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 66 - Plus One 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/plus-one/>  

![](/assets/img/sample/leetcode/66/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/66/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 벡터로 들어온 값을 하나의 정수로 봅니다. [1,2,3]은 123처럼 봅니다.
- 바뀐 정수에 +1을 한 값을 리턴합니다.
- 999,9999인 경우만 조심하면 어려울 게 없는 문제입니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int carry= 0;
        int size = digits.size()-1;
        while(size>=0 && digits[size]==9)
        {
            digits[size]=0;
            --size;
        }
        //만약 999,9999처럼 끝까지 9만 반복된 경우 뒤에 1을 넣고 반전시켜버립니다. 00001->10000처럼
        if(size==-1)
        {
            digits.push_back(1);
            reverse(digits.begin(),digits.end());
        }
        else
            digits[size]++;
        return digits;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/66/result.JPG)  






