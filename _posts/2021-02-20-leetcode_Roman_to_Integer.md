---
title: leetcode(리트코드)2월20일 challenge13-Roman to Integer
author: 강민석
date: 2021-02-20 01:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge13 - Roman to Integer 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/roman-to-integer/>  

![](/assets/img/sample/leetcode/13/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/13/input.JPG)  

Constraints:

- 1 <= s.length <= 15
- s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').
- It is guaranteed that s is a valid roman numeral in the range [1, 3999].

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월20일자 챌린지 문제입니다. 

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

**시간(43%)**


![](/assets/img/sample/leetcode/13/result.JPG)  


**0ms시간코드(100%)**

```c++
class Solution {
    int charToInt(char c) {
        if (c == 'I') return 1;
        else if (c == 'V') return 5;
        else if (c == 'X') return 10;
        else if (c == 'L') return 50;
        else if (c == 'C') return 100;
        else if (c == 'D') return 500;
        else return 1000;
    }
public:
    int romanToInt(string s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            int num1 = charToInt(s[i]), num2 = 0;
            if (i + 1 < n)
                num2 = charToInt(s[i + 1]);
            
            if (num1 < num2) {
                ans += (num2 - num1);
                i++;
            }
            else {
                ans += num1;
            }
        }
        return ans;
    }
};
```

코드 본문의 내용이 쉬우므로 해설은 적지 않겠습니다.  

