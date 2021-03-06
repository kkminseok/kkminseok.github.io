---
title: Programmers_BFS/DFS02 - 네트워크
author: 강민석
date: 2021-02-05 15:03:00 +0800
categories: [Algorithm,DFS]
tags: [Programmers,DFS]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 네트워크 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/BDFS02/Problem.JPG)  



-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 방문처리를 해줘야한다는 점 외에는 신경쓸 일이 없습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<queue>
#include<iostream>
using namespace std;

bool check[201] = { false, };
void DFS(int start,int n, vector<vector<int>> computers)
{
    check[start] = true;
    for (int i = 0; i < n; ++i)
    {
        if (!check[i] && computers[start][i])
            DFS(i, n, computers);
    }
}

int solution(int n, vector<vector<int>> computers) {
    int answer = 0;
    for (size_t i = 0; i < n; ++i)
    {
        if (!check[i])
        {
            ++answer;
            DFS(i, n, computers);
        }
    }

    return answer;
}

```
-----

## 5. 결과

![](/assets/img/sample/Programmers/BDFS02/result.JPG)  













 
