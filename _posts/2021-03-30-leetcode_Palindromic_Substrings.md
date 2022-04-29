---
title: leetcode(리트코드)3월27일 challenge647-Palindromic Substrings
author: 강민석
date: 2021-03-30 00:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 27일 - Palindromic Substrings 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/palindromic-substrings/>  

![](/assets/img/sample/leetcode/647/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/647/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 27일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 문자열 속에 있는 모든 문자열에 대해 Palindromic이 되는 지 확인하고 개수를 구합니다.
- 저는 brute하게 풀어서 좋은 코드는 아닙니다. 밑에 좋은 코드를 따로 첨부하겠습니다.




-----  

## 5. code

**내가 푼 c++**

```c++
class Solution {
public:
    bool check(string s,int left,int right){
        while(left<=right){
            if(s[left]!=s[right])
                return false;
            ++left;
            --right;
        }
        return true;
    }
    int countSubstrings(string s) {
        int result = 0;
        for(size_t i =0;i<s.size();++i){
            for(size_t j = i ;j<s.size();++j){
                if(check(s,i,j)){
                    ++result;
                }
            }
        
        return result;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 5%**
![](/assets/img/sample/leetcode/647/result.JPG)  


**개선한 코드 4ms 87%**

```c++
class Solution {
public:
    int countSubstrings(string s) {
        int leng = s.size();
        int count = 0;
        
        for(int i =0;i<leng;++i){
            //홀수길이의 s
            palindromic(s,i,i,count);
            //짝수길이의 s
            palindromic(s,i,i+1,count);
        }
        return count;
    }
    void palindromic(string s,int left,int right,int& count){
        while(0<=left && right<s.size() && s[left] == s[right]){
            ++count;
            --left;
            ++right;
        }
    }
};
```

값을 기준으로 양쪽으로 뻗어나가 계산하는 코드입니다. 