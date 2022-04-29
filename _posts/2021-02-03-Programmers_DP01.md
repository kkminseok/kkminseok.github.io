---
title: Programmers_DP01 - N으로 표현
author: 강민석
date: 2021-02-03 12:06:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,DP]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - N으로 표현 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/DP01/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- DP로 푼다기 보다 DFS로 풀 수 있어서 DFS로 풀었습니다.
- 아마 반복되는 연산들을 DP로 풀면 시간단축에 도움이 될 것 같습니다.   
  범위가 어디까지인지를 안정해줘서 DP로 풀려다가 실패했습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<queue>
#include<cmath>
using namespace std;

const int MAX = 9;
int minD = MAX;


void DFS(int N, int number,int depth, int num)
{
    if (depth >= MAX)
    {
        return;
    }
    if (num == number)
    { 
        minD = min(depth, minD);
    }

    int op = 0;
    for (int i = 1; i <= MAX; ++i)
    {
        op = op * 10 + N;
        DFS(N, number, depth + i, num + op);
        DFS(N, number, depth + i, num - op);
        if (num != 0)
        {
            DFS(N, number, depth + i, num * op);
            DFS(N, number, depth + i, num / op);
        }
    }
    
   
}

int solution(int N, int number) {
    int answer = 0;

    DFS(N, number,0,0);
    if (minD > 8)
        answer = -1;
    else
        answer = minD;
    return answer;
}
int main()
{
    solution(5, 12);
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/DP01/result.JPG)  













 
