---
title: leetcode(리트코드)221-Maximal Square
author: 강민석
date: 2021-03-30 01:01:10 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 221 - Maximal Square 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximal-square/>  

![](/assets/img/sample/leetcode/221/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/221/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 1로 이루어진 정사각의 넓이가 가장 큰 것의 넓이를 리턴하세요.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int DP[301][301]={0,};
        int h = matrix.size();
        int w = matrix[0].size();
        int result= 0;
        for(int i =0;i<h;++i){
            for(int j =0;j<w;++j){
                if(i==0 || j==0 || matrix[i][j]=='0')
                    DP[i][j] = matrix[i][j]-48;
                else
                {
                    DP[i][j] = min(DP[i-1][j-1],min(DP[i-1][j],DP[i][j-1])) +1;
                }
                result= max(result,DP[i][j]);
            }
        }
        return result*result;
        
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 60% python ??%** 
![](/assets/img/sample/leetcode/221/result.JPG)  

