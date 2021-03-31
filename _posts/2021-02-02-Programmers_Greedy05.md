---
title: Programmers_Greedy05 - 구명보트
author: 강민석
date: 2021-02-02 14:03:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Greedy]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 구명보트 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Greedy05/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 퀵소트처럼 맨 뒤와 앞에서 접근하며 담을 수 없는건 한명만 담고 담을 수 있으면 담아버립니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<iostream>
using namespace std;



int solution(vector<int> people, int limit) {
    int answer = 0;
    int exitif = 0;
    sort(people.begin(), people.end());
    int maxn = people.size() - 1;
    int minn = 0;
    while (minn <= maxn)
    {
        if (people[minn] + people[maxn] <= limit)
        {
            ++minn;
            --maxn;
        }
        else
        {
            --maxn;
        }
        answer++;
    }
    
    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Greedy05/result.JPG)  













 
