---
title: leetcode(리트코드)125-Valid Palindrome
author: 강민석
date: 2021-04-01 00:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 125 - Valid Palindrome 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/valid-palindrome/>  

![](/assets/img/sample/leetcode/125/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/125/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 문자열이 들어오는데, 영문과 숫자만 검사했을 때 회문이 되는 지 검사합니다.
- 특수문자, 공백은 제외합니다. 

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    
    bool isPalindrome(string s) {
        int left = 0;
        int right = s.size()-1;
        
        //양방향으로 검사합니다.
        while(left<=right){
            //영문, 숫자가아니면 건너뜁니다.
            while(left< s.size() && !isd(s[left]) && !ise(s[left])){
                ++left;
            }
            //마찬가지로 건너뜁니다.
            while(right> 0 && !isd(s[right]) && !ise(s[right])){  
                --right;
            }
            //만약 영문숫자가 하나도 안들어온 경우 true
            if(left>right)
                return true;
            //영문자라면 소문자로 바꿔서 비교
            if(ise(s[left]) && ise(s[right]) ){
                s[left] = tolower(s[left]);
                s[right] = tolower(s[right]);
            }
            if(s[left] != s[right])
                return false;
            //cout<<s[left]<<" "<<s[right]<<'\n';
            ++left;
            --right;
            
        }
        return true;
    }
    bool isd(char c){
        return isdigit(c);
    }
    bool ise(char c){
        return isalpha(c);
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 52% python ??%** 
![](/assets/img/sample/leetcode/125/result.JPG)  






