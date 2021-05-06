---
title: leetcode(리트코드)58-Length of Last Word
author: 강민석
date: 2021-05-03 07:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 58 - Length of Last Word 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/length-of-last-word/> 

![](/assets/img/sample/leetcode/58/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/58/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Random으로 뽑아서 푼 문제입니다.
좋은 문제는 아닙니다. 


-----  

## 4. 문제 해석

- 문자열에서 마지막의 부분문자열의 길이를 리턴합니다.



-----  

## 5. code  

코드설명
- ' ' 이 들어올 경우 count값을 0으로 만들어줍니다.

**c++**

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int count = 0;
        int res = 0 ;
        for(int i = 0 ; i<s.size();++i){
            if(s[i] ==' '){
                count=0;
                continue;
            }
            ++count;
            res =count;
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 60% 4ms**

![](/assets/img/sample/leetcode/58/result.JPG)  




