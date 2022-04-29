---
title: leetcode(리트코드)190-Reverse Bits
author: 강민석
date: 2021-03-22 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 190 - Reverse Bits 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reverse-bits/>  

![](/assets/img/sample/leetcode/190/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/190/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- 문제로 들어온 값을 2진수로 바꾸고 역순으로 바꾼 숫자를 리턴합니다.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        //bitset 
        bitset<32> bs;
        int count = 0 ;
        //들어온 값을 2진수로 바꾸는 과정
        while(n!=0)
        {
            bs.set(count,n%2);
            n/=2;
            ++count;
        }
        //bitset을 다시 문자열로 바꾸고
        string result = bs.to_string();
        //역순으로 바꿔버립니다.
        reverse(result.begin(),result.end());
        //2진수를 다시 숫자로 바꾸기 위해서 bitset에 다시 넣고
        bitset<32> resultbs(result);
        //숫자로 바꿔버립니다.
        uint32_t find = resultbs.to_ulong();
        
        return find;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 56% python ??%** 
![](/assets/img/sample/leetcode/146/result.JPG)  






