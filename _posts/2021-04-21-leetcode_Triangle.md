---
title: leetcode(리트코드)4월21일 challenge120-Triangle
date: 2021-04-21 10:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 21일 - Triangle 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/triangle/>  

![](/assets/img/sample/leetcode/120/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/120/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 21일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 삼각형이 주어집니다. 삼각형의 길을 하나 따라서 밑으로 내려가는데, 그 값들의 합이 최소가 되는 값을 찾아 리턴하세요.




-----  

## 5. code

DP문제이고, 길을 따라 내려가면서 위쪽의 최솟값만 찾아주면되므로 어려운 문제는 아닙니다.

**c++**

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        if(triangle.size() == 1) return triangle[0][0];
        int res = INT_MAX;
        int DP[201][201]={0,};
        DP[0][0] = triangle[0][0];
        for(int i = 1 ;i<triangle.size();++i){
            for(int j = 0; j<= i;++j){
                
                if(j ==0){
                    DP[i][j] = DP[i-1][j] + triangle[i][j];
                }
                else if(j==i){
                    DP[i][j] = DP[i-1][j-1] + triangle[i][j];
                }
                else{
                    DP[i][j] = min(DP[i-1][j-1],DP[i-1][j]) + triangle[i][j];
                }
                if(i == triangle.size()-1){
                    res = min(res,DP[i][j]);
                }
            }
            
        }
        return res;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/120/result.JPG)  




