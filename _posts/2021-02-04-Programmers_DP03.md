---
title: Programmers_DP03 - 등굣길
author: 강민석
date: 2021-02-04 12:03:00 +0800
categories: [Algorithm,DP]
tags: [Programmers,DP]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 등굣길 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/DP03/Problem.JPG)  
![](/assets/img/sample/Programmers/DP03/Problem2.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 비슷한 문제가 백준에 있습니다. 그 코드를 참고해서 DFS로 풀었습니다.  
- 마지막 결과값에 나머지연산을 안해줘서 틀렸었습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<cstring>
using namespace std;

const int MAX = 201;

int MAP[MAX][MAX] = { 0, };
int v[MAX][MAX] = { 0, };

int dx[2] = {1,0 };
int dy[2] = {0,1 }; 
int DFS(int i, int j, int m, int n)
{
    if (i == n - 1 && j == m - 1)
    {
        return 1;
    }
    if (v[i][j] != -1)
        return v[i][j];
    v[i][j] = 0;
    for (int k = 0; k < 2; ++k)
    {
        int newX = i + dx[k];
        int newY = j + dy[k];
        if (0 <= newX && newX < n && 0 <= newY && newY < m && MAP[newX][newY] != 1)
           v[i][j] += (DFS(newX, newY, m, n)% 1000000007);
    }
    return v[i][j]% 1000000007;
}

int solution(int m, int n, vector<vector<int>> puddles) {
    int answer = 0;
    for (size_t i = 0; i < puddles.size(); ++i)
    {
        MAP[puddles[i][1]-1][puddles[i][0]-1] = 1;
    }
    memset(v, -1, sizeof(v));
    answer = DFS(0, 0,m,n);

    return answer;
}
int main()
{
    vector<vector<int>> puddles(10,vector<int>(10,0));
    puddles[2].push_back(2);
    solution(4, 3, puddles);

}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/DP03/result.JPG)  













 
