---
title: leetcode(리트코드)14-Longest Common Prefix
author: 강민석
date: 2021-03-22 01:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 14 - LongestCommon Prefix 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-common-prefix/>  

![](/assets/img/sample/leetcode/14/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/14/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- input으로 여러 문자열이 들어옵니다.
- 들어온 문자열에서 가장 긴 공통 접두사를 찾아 리턴합니다.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size()==0)
            return "";
        
        string result="";
        bool ok = false;
        //인덱스를 접근하는 변수입니다.
        int count = 0; 
        //어느 문자열의 끝을 돌았거나, 문자열의 인덱스가 다른 것을 찾습니다.
        while(!ok)
        {
            //기준이 되는 문자로 가장 첫번째 문자열을 기준으로합니다.
            char standard;
            //count가 문자열의 범위를 벗어나지 않게 조정합니다.
            if(count < strs[0].size())
                standard = strs[0][count];
            for(size_t i =0;i<strs.size();++i)
            {
                //ok를 관리하기 위한 구문입니다.
                if(strs[i][count]!=standard || count>= strs[i].size())
                {
                    ok=true;
                    break;
                }

            }
            if(!ok)
            {
                ++count;
                result+=standard;
            }
        }
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/14/result.JPG)  






