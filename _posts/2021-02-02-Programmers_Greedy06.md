---
title: Programmers_Greedy06 - 단속카메라
author: 강민석
date: 2021-02-02 15:03:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Greedy]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 단속카메라 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Greedy06/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 생각은 어렵지 않았으나 코드에 담는데 시간이 걸렸습니다.
- 차가 지나가고 나서 새로 오면 일단 카메라를 설치하는 것으로 경계선을 구분했습니다.
- 마지막에 1을 더해준건 마지막에 카메라가 더해지지 않기 때문입니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
using namespace std;

int solution(vector<vector<int>> routes) {
    int answer = 0;
    sort(routes.begin(), routes.end());
    int stop = routes[0][1];
    for (int i = 1; i < routes.size(); ++i)
    {
        if (stop < routes[i][0])
        {
            ++answer;
            stop = routes[i][1];
        }
        if (stop >= routes[i][1]) stop = routes[i][1];
    }

    return answer+1;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Greedy06/result.JPG)  













 
