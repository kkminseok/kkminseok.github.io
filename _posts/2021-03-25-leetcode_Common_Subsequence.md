---
title: leetcode(리트코드)1143-Common Subsequence
author: 강민석
date: 2021-03-25 07:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1143 - Common Subsequence 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-common-subsequence/>  

![](/assets/img/sample/leetcode/1143/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1143/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- DP문제입니다.
- 문자열 2개에서 가장 긴 공통된 문자열을 찾아 그 길이를 리턴합니다.



-----  

## 5. code


**c++**
```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        //DP테이블
        int** check;
        // row는 text1의 길이보다 1만큼 증가
        int row = text1.size()+1;
        // col도 마찬가지. 이유는 초기값을 0으로 셋팅하기 위해서
        int col = text2.size()+1;
        check=new int*[row * sizeof(int)];
        for(int i =0;i<row;++i)
            check[i] = new int[col * sizeof(int)];
        for(int i = 0;i<row;++i)
        {
            for(int j = 0 ;j<col;++j)
            {
                //초기값 세팅
                if(i==0 || j==0 )
                {
                    check[i][j] = 0;
                    continue;
                }
                //점화식 부분.
                if(text1[i-1] ==text2[j-1])
                {
                    check[i][j]= check[i-1][j-1]+1;
                }
                else
                    check[i][j] = max(check[i-1][j],check[i][j-1]);
            }
        }
        return check[row-1][col-1];
            
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 74% python ??%** 
![](/assets/img/sample/leetcode/1143/result.JPG)  






