---
title: leetcode(리트코드)28-Implement strStr()
author: 강민석
date: 2021-03-23 02:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 28 - Implement strStr() 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/implement-strstr/>  

![](/assets/img/sample/leetcode/28/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/28/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 동일한 문자열을 찾습니다.
- 예외처리가 중요한 문제라고 적혀있습니다.
- 연습할 겸 KMP알고리즘을 이용해서 풀었습니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(haystack.empty()&& needle.empty())return 0;
        else if(!haystack.empty() && needle.empty())return 0;
        
     return KMP(haystack,needle);
    }
    vector<int> maketable( string pattern)
    {
        int parttsize = pattern.size();
        vector<int> table(parttsize,0);
        int j  =0;
        for(int i =1;i<pattern.size();++i)
        {
            while(j>0 && pattern[i] != pattern[j])
            {
                j = table[j-1];
            }
            if(pattern[i] == pattern[j])
                table[i] = ++j;
        }
        return table;
    }
    
    int KMP(string origin, string pattern)
    {
        vector<int> table = maketable(pattern);
        int originsize = origin.size();
        int pattsize= pattern.size();
        int j =0;
        for(int i =0;i<origin.size();++i)
        {
            while(j> 0 && origin[i] != pattern[j])
                j = table[j-1];
            if(origin[i] == pattern[j])
            {
                if(j==pattsize-1)
                    return i - pattsize + 1;
                else
                    ++j;
            }
        }
        return -1;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 73% python ??%** 
![](/assets/img/sample/leetcode/28/result.JPG)  






