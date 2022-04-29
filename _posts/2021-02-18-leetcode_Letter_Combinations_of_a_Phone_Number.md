---
title: leetcode(리트코드)17-Letter Combinations of a Phone Number
author: 강민석
date: 2021-02-18 13:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 17 - Letter Combinations of a Phone Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/letter-combinations-of-a-phone-number/>  

![](/assets/img/sample/leetcode/17/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/17/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 전화기에 영어가 붙어있습니다. digits로 들어온 수에 적혀있는 영어들로 만들 수 있는 경우의 string들을 리턴합니다.



-----  

## 5. code

```c++
vector<string> alpha(10);
class Solution {
public:
    void getR(vector<string>& result,string digits,int count)
    {
        if(count==digits.size())
        {
            result.push_back(alpha[0]);   
        }
        else
        {
            for(int i=0;i<alpha[digits[count]-48].size();++i)
            {
                alpha[0]+=alpha[digits[count]-48][i];
                getR(result,digits,count+1);
                alpha[0].resize(size(alpha[0])-1);
            }
        }
    }
    vector<string> letterCombinations(string digits) {
        alpha[0]="";
        alpha[1]="";
        alpha[2]="abc";
        alpha[3]="def";
        alpha[4]="ghi";
        alpha[5]="jkl";
        alpha[6]="mno";
        alpha[7]="pqrs";
        alpha[8]="tuv";
        alpha[9]="wxyz";
        vector<string> result;
        if(digits.size()==0)
           return result;
        getR(result,digits,0);
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(100%)**  

![](/assets/img/sample/leetcode/17/result.JPG)

