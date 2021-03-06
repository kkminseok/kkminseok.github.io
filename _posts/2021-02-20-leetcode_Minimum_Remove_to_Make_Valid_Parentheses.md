---
title: leetcode(리트코드)2월19일 challenge1249-Minimum Remove to Make Valid Parentheses
author: 강민석
date: 2021-02-20 00:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge19 - Minimum Remove to Make Valid Parentheses 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/>  

![](/assets/img/sample/leetcode/1249/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1249/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월19일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 문자가 들어옵니다. 들어오는 문자가 단어가 되도록 만들어야합니다. 괄호의 짝이 맞아야합니다.

-----  

## 5. code

```c++
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        stack<int> st;
        for(size_t i=0;i<s.size();++i)
        {
            if(s[i]=='(')
            {
                st.push(i);
            }
            else if(s[i]==')')
            {
                if(st.empty())
                    s[i]='*';
                else
                    st.pop();
            }
        }
        while(!st.empty())
        {
            s[st.top()]='*';
            st.pop();
        }
        
        s.erase(remove(s.begin(),s.end(),'*'),s.end());
        
        return s;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**시간(95%)**

Discuss를 참조했습니다.

![](/assets/img/sample/leetcode/1249/result.JPG)  


