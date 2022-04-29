---
title: leetcode(리트코드)387-First Unique Character in a String
author: 강민석
date: 2021-04-19 11:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 387 - First Unique Character in a String 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/first-unique-character-in-a-string/>  

![](/assets/img/sample/leetcode/387/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/387/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 문자열에서 한 번만 반복되는 문자를 찾아 index를 리턴합니다.
- 한 번만 반복되는 문자가 없으면 -1을 리턴합니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char,int> um;
        
        for(int i = 0 ; i <s.size();++i){
            um[s[i]]++;
        }
        for(int i = 0;i<s.size();++i){
            if(um[s[i]] == 1)
                return i;
        }
        return -1;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 44% python ??%** 
![](/assets/img/sample/leetcode/387/result.JPG)  






