---
title: leetcode(리트코드)3월08일 challenge1332-Remove Palindromic Subsequences
author: 강민석
date: 2021-03-09 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 08일 - Remove Palindromic Subsequences 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-palindromic-subsequences/>  

![](/assets/img/sample/leetcode/1332/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1332/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
3월 08일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 좋은 문제는 아닌 것 같습니다. Palindromic이면 1을 리턴 아니면 2를 리턴 문자열의 크기가 0인 경우 0을 리턴하는 문제입니다.



-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int removePalindromeSub(string s) {
        if(s.size()==0)
            return 0;
        string reverseS = s;
        std::reverse(reverseS.begin(),reverseS.end());
        if(s == reverseS)
            return 1;   
        return 2;
    }
};
```

-----  

**python**

```python
class Solution:
    def removePalindromeSub(self, s: str) -> int:
        if(len(s)==0):
            return 0
        reverseS = s[::-1]
        if(reverseS == s):
            return 1
        else :
            return 2
```

-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1332/result.JPG)  




