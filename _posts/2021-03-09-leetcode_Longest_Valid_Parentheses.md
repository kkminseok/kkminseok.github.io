---
title: leetcode(리트코드)32-Longest Valid Parentheses
author: 강민석
date: 2021-03-09 01:00:00 +0800
categories: [leetcode,Hard]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 32 - Longest Valid Parentheses 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-valid-parentheses/>  

![](/assets/img/sample/leetcode/32/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/32/input.JPG)  

-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- "()"짝을 가진 가장 긴 길이의 substring의 길이를 반환합니다.
- 2시간 정도 잡고 있다가 안 풀려서 Discuss를 참조하였습니다.


-----  

## 5. code

**python**


```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stk = []
        stk.append(-1)
        ans = 0
        for i in range(len(s)):
            if s[i] == '(' : 
                stk.append(i)
            else:
                stk.pop()
                if len(stk)==0 : 
                    stk.append(i)
                else:
                    ans = max(ans,i - stk[-1])
        return ans
```
        
-----  

**c++** 


```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> stk;
        stk.push(-1);
        int ans = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') {
                stk.push(i);
            } else {
                stk.pop();
                if (stk.empty()) {
                    stk.push(i);
                } else {
                    ans = max(ans, i - stk.top());
                }
            }
        }
        return ans;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/32/result.JPG)  


