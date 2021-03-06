---
title: leetcode(리트코드)20-Valid Parentheses
author: 강민석
date: 2021-02-18 14:10:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 20 - Valid Parentheses 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/valid-parentheses/>  

![](/assets/img/sample/leetcode/20/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/20/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 스택으로 푸는 유명한 괄호문제입니다. 짝이 맞으면 true 틀리면 false를 리턴합니다.



-----  

## 5. code

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for(size_t i =0;i<s.size();++i)
        {
            if(s[i]=='(' || s[i]=='{' || s[i]=='[')
            {
                st.push(s[i]);
            }
            else
            {
                if(st.empty())
                    return false;
                else
                {
                    char temp = st.top();
                    st.pop();
                    if(s[i]==')' && temp!='(')
                        return false;
                    else if(s[i]==']' && temp!='[')
                        return false;
                    else if(s[i]=='}' && temp!='{')
                        return false;       
                }
            }
        }
        if(st.empty())
            return true;
        else
            return false;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(100%)**  

![](/assets/img/sample/leetcode/20/result.JPG)



