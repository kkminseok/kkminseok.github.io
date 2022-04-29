---
title: leetcode(리트코드)91-Decode Ways
author: 강민석
date: 2021-03-30 12:34:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 91 - Decode Ways 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/decode-ways/>  

![](/assets/img/sample/leetcode/91/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/91/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- DP문제입니다.
- 프로그래머스에 비슷한 문제가 있었던 것 같은데..
- '0'에 대한 처리를 잘 해주면 특이하게 어려울 건 없는 문제입니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int numDecodings(string s) {   
        if(s[0]=='0')
            return 0;
        if(s.size()==1) {
            return 1;
        }
        int* DP= new int[s.size()+1];
        DP[0]=1;
        DP[1]=1;
        for(size_t i =2;i<=s.size();++i){
            if(s[i-1]!='0')
                DP[i] = DP[i-1];
            else
                DP[i]=0;
            string temp ="";        
            temp+=s[i-2];
            temp+=s[i-1];
            int con = stoi(temp);
            if(con<=26 && con>=10)
                DP[i] += DP[i-2];  
        }
        
        return DP[s.size()];
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/91/result.JPG)  






