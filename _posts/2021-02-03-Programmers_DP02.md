---
title: Programmers_DP02 - 정수 삼각형
author: 강민석
date: 2021-02-03 12:03:00 +0800
categories: [Algorithm,DP]
tags: [Programmers,DP]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 정수 삼각형 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/DP02/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 비슷한 문제가 백준에 있어서 푸는데 수월했습니다.  

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<iostream>
using namespace std;
int dp[501][501] = { 0, };


int solution(vector<vector<int>> triangle) {
    int answer = 0;
    dp[0][0] = triangle[0][0];
    for (size_t i = 1; i < triangle.size(); ++i)
    {
        for (int j = 0; j < triangle[i].size(); ++j)
        {
            if (j==0)
            {
                dp[i][j] = dp[i - 1][j] + triangle[i][0];
               
            }
            else
            {
                dp[i][j] += max(dp[i - 1][j - 1]+ triangle[i][j], dp[i - 1][j] + triangle[i][j]);
            }
            answer = max(dp[i][j], answer);
        }
    }
    
    
    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/DP02/result.JPG)  













 
