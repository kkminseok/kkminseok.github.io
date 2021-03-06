---
title: leetcode(리트코드)10-Regular Expression Matching
author: 강민석
date: 2021-03-05 00:00:00 +0800
categories: [leetcode,Hard]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 10 - Regular Expression Matching 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/regular-expression-matching/>  

![](/assets/img/sample/leetcode/10/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/10/input.JPG)  

**Constraints:**

- 0 <= s.length <= 20
- 0 <= p.length <= 30
- s contains only lowercase English letters.
- p contains only lowercase English letters, '.', and '*'.
It is guaranteed for each appearance of the character '*', there will be a previous valid character to match.


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Interviewd 문제입니다.  


-----  

## 4. 문제 해석

- stoi()를 해주는 문제이지만.. 예외처리할 게 많습니다.



-----  

## 5. code

```c++
class Solution {
public:
    int myAtoi(string s) {
        if(s[0]>96 && s[0]<123 || s[0]=='.')
            return 0;
        //맨 처음이 공백인 경우 공백이 아닐때까지 점프합니다.
        if(s[0]==' ')
        {
            int i = 0;
            while(i<s.size() && s[0]==' ')
            {
                s = s.substr(1);
            }
        }
        int result;
        try
        {
            result = std::stoi(s);
        }
        //문자가 들어온 경우
        catch(invalid_argument& e)
        {
            return 0;
        }
        //오버플로우, 언더플로우인 경우
        catch(std::out_of_range& e)
        {
            if(s[0]=='-')
                result = -2147483648;
            else
                result = 2147483647;
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/10/result.JPG)  


