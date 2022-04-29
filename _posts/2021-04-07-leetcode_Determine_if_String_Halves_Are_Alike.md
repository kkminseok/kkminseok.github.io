---
title: leetcode(리트코드)4월07일 challenge1704-Determine if String Halves Are Alike
author: 강민석
date: 2021-04-07 07:00:56 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 07일 - Determine if String Halves Are Alike 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/determine-if-string-halves-are-alike/>  

![](/assets/img/sample/leetcode/1704/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1704/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
4월 07일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 문자열이 들어옵니다. 반 반으로 나눈 문자열의 모음 갯수가 같으면 true 다르면 false를 리턴합니다.





-----  

## 5. code

**c++**

```c++
class Solution {
public:
    bool halvesAreAlike(string s) {
        string a,b;
        set<char> st;
        st.insert('a');
        st.insert('e');
        st.insert('i');
        st.insert('o');
        st.insert('u');
        a = s.substr(0,s.size()/2);
        b = s.substr(s.size()/2);
        int aj = 0;
        int bj = 0;
        for(size_t i = 0 ;i<a.size();++i){
            if(st.count(tolower(a[i])))++aj;
            if(st.count(tolower(b[i])))++bj;
        }
        return aj==bj;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 100%**
![](/assets/img/sample/leetcode/1704/result.JPG)  


