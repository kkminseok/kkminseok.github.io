---
title: leetcode(리트코드)2월11일 challenge242-Valid Anagram
author: 강민석
date: 2021-02-11 16:30:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,String]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge11 - Valid Anagram 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/valid-anagram/>  

![](/assets/img/sample/leetcode/242/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/242/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월11일자 챌린지 문제입니다.   

-----  

## 4. 문제 해석

- S로 주어진 문자열을 문자순서를 바꿔 T를 만들 수 있는지 묻는 문제입니다. 

-----  

## 5. code

```c++
class Solution {
public:
    int alpha[26]={0,};
    bool isAnagram(string s, string t) {
        if(s.size()!=t.size())
            return false;
            
        for(size_t i=0;i<s.size();++i)
        {
            alpha[s[i]-97]++;
        }
        for(size_t i=0;i<t.size();++i)
        {
            if(alpha[t[i]-97]-1 <0)
                return false;
            alpha[t[i]-97]--;
        }
        return true;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/242/result.JPG) 


**시간효율성이 좋은 코드**

제 코드와 비슷합니다. (98%) 

외에 문자열을 둘 다 정렬해줘서 틀린 첨자가 있으면 false를 리턴하는 방법이 있습니다.  
