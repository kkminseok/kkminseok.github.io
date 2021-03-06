---
title: leetcode(리트코드)2월24일 challenge856-Score of Parentheses
author: 강민석
date: 2021-02-24 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 856 - Scroe of Parentheses 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/score-of-parentheses/>  

![](/assets/img/sample/leetcode/856/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/856/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월24일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- input으로 "()"짝이 들어옵니다. 기본적으로 ()는 1로 치환됩니다.
- 괄호 안에 괄호가 들어 있으면 > "(())" 괄호안의 숫자의 2배를 해줍니다.
- "()()" 이렇게 이어져 있으면 () + () 를 해야합니다.

-----  

## 5. code

```c++
class Solution {
public:
    int scoreOfParentheses(string S) {
        int result = 0;
        int count = 0;
        bool check  = false;
        for(size_t i = 0;i<S.size();++i)
        {
            if(S[i] == '(')
            {
                ++count;
                check=true;
            }
            else if (S[i]==')' && check)
            {
                result += pow(2,count-1);
                --count;
                check=false;
            }
            else
            {
                --count;
            }
        }
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/856/result.JPG)  

