---
title: leetcode(리트코드)416-Partition Equal Subset Sum
author: 강민석
date: 2021-04-14 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 416 - Partition Equal Subset Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/decode-string/>  

![](/assets/img/sample/leetcode/394/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/394/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- string이 들어옵니다. 숫자만큼 '[]' 에 있는 것들을 반복해야합니다.
- 결과를 리턴합니다.
- discuss 참조했습니다.


-----  

## 5. code


**c++**


```c++

class Solution {
public:
    string decodeString(string s) {
        stack<string> chars;
        stack<int> nums;
        int num = 0;
        string res = "";
        
        for(char c : s){
            if(isdigit(c)){
                num = num * 10 + (c-'0');
            }
            else if(isalpha(c)){
                res.push_back(c);
            }
            else if(c =='['){
                chars.push(res);
                nums.push(num);
                num = 0;
                res = "";
            }
            else if(c==']'){
                string tmp = res;
                for(int i =0;i<nums.top()-1; ++i)
                    res += tmp;
                res = chars.top() + res;
                chars.pop(), nums.pop();
            }

        }
        return res;
    }
};

```


-----

## 6. 결과 및 후기, 개선점


**c++ 100%**


![](/assets/img/sample/leetcode/394/result.JPG)  





