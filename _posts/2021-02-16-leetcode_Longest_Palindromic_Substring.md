---
title: leetcode(리트코드)5-Longest Palindromic Substring
author: 강민석
date: 2021-02-16 15:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 5 - Longest Palindromic Substring 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-palindromic-substring/>  

![](/assets/img/sample/leetcode/5/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/5/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 부분 문자열의 회문을 찾는데 가장 길이가 긴 것을 찾아 리턴합니다.
- 어려워서 discuss를 봤습니다.



-----  

## 5. code

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        
        //사이즈가 작은게 들어오면
        if (s.size()<=1) 
            return s;
        int min_left=0;
        int max_len=1;
        int max_right=s.size()-1;
        //인덱스 mid를 기준으로 양쪽으로 검사를 한다.
        for (int mid=0;mid<s.size();){
            int left=mid;
            int right=mid;
            //right는 오른쪽으로 가면서 검사하는데 +1과 같으면 오른쪽으로 이동해서 max_len값을 늘린다. 같으면 어쨋든 palindromic이다.
            while (right<max_right && s[right+1]==s[right])
                right++; // Skip duplicate characters in the middle
            //right로만으 일단 palindromic을 구했으므로 다음 루프때는 그보다 1 큰값에서 루프를 돌게 한다.
            mid=right+1; //for next iter
            //왼쪽으로 이동하면서 검사한다.
            while (right<max_right && left>0 && s[right+1]==s[left-1]){
                right++; 
                left--;
            } // Expand the selection as long it is a palindrom
            int new_len=right-left+1; //record best palindro
            cout<<new_len<<"\n";
            if (new_len>max_len){ 
                min_left=left; 
                max_len=new_len; 
            }
        }
        return s.substr(min_left, max_len);

    }
};
```


-----

## 6. 결과 및 후기, 개선점

discuss를 봤으므로 올리지 않습니다.  