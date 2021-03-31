---
title: Programmers_Heap01 - 더 맵게
author: 강민석
date: 2021-01-27 11:09:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Heap]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 더 맵게 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/HEAP_01/Problem.JPG)  


- 섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 처음 두 개의 값을 빼서 계산한 다음 다시 넣는데 정렬해서 넣어야하므로 우선순위 큐를 생각했습니다.
- 예외는 두 가지 상황을 고려했습니다.
    + 처음에 아예 안들어올 때
    + 첫 번째 요소를 큐에서 제거했는데 다음에 빼야할 두 번째 요소가 없을 때
- 큐 맨 앞에 있는 요소가 K를 넘을 때 반복문을 벗어납니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <algorithm>
#include<queue>
#include <iostream>


using namespace std;

int solution(vector<int> scoville, int K) {
    int answer = 0;
    priority_queue<int, vector<int>, greater<int>> q;
    for (size_t i = 0; i < scoville.size(); ++i)
    {
        q.push(scoville[i]);
    }
    while (!q.empty() &&q.top() <= K )
    {
        int Nspicy = q.top();
        q.pop();
        if (q.empty())
        {
            answer = -1;
            break;
        }
        int Nspicy2 = q.top();
        q.pop();
        int mix = Nspicy + (Nspicy2 * 2);
        q.push(mix);
        ++answer;
    }
    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/HEAP_01/result.JPG)











 