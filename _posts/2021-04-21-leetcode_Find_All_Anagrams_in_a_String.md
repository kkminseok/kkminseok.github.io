---
title: leetcode(리트코드)438-Find All Anagrams in a String
author: 강민석
date: 2021-04-21 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 438 - Find All Anagrams in a String 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-all-anagrams-in-a-string/> 

![](/assets/img/sample/leetcode/438/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/438/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- s라는 문자열과 p라는 문자열이 주어집니다.
- p에 있는 문자열이 순서와 상관없이 전부 들어있는 부분문자열의 첫 인덱스 위치를 vector에 넣어 리턴합니다.



-----  

## 5. code  

- 핵심 아이디어는 문자열의 위치를 1만큼 뒤로가면서 새로 추가된 문자에 대한 카운트를 증가시켜주고 맨앞의 문자에대한 카운트를 지워줍니다. 그리고 비교를 합니다.


**c++**

```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> res;
        if(p.size() > s.size()) return res;
        vector<int> pv(26,0),sv(26,0);
        for(int i = 0; i<p.size(); ++i){
            pv[p[i]-'a']++;
            sv[s[i]-'a']++;
        } 
        if(pv==sv) res.push_back(0);
        
        for(int i = p.size() ; i<s.size();++i){
            sv[s[i]-'a']++;
            sv[s[i-p.size()]- 'a']--;
            if(sv==pv)res.push_back(i-p.size()+1);
        }
        
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 73% 12ms**

![](/assets/img/sample/leetcode/438/result.JPG)  



