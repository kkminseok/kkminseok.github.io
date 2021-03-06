---
title: Programmers_Stack/Queue03 - 다리를 지나는 트럭
author: 강민석
date: 2021-01-26 15:07:00 +0800
categories: [Algorithm,Queue]
tags: [Programmers,Queue]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -스택/큐 - 다리를 지나는 트럭 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/SQ_03/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
스택 큐 Level 2 문제 입니다.    

-----  

## 3. 생각한 것들(문제 접근 방법)

- 처음에는 들어갈 수 있는 최대한을 큐에 다 넣고 시간을 증가시켰는데 틀렸다고 나왔습니다.  


```c++
다리를 지나가는 트럭
#include <string>
#include <vector>
#include <queue>
#include<algorithm>
#include<iostream>
using namespace std;

int solution(int bridge_length, int weight, vector<int> truck_weights) {
    int answer = 0;
    queue<pair<int, int>> truckQ;
    int index = 0;
    while (index < truck_weights.size() || !truckQ.empty())
    {
        while (weight >= truck_weights[index] && index < truck_weights.size())
        {
            truckQ.push(make_pair(truck_weights[index],++answer));
            weight -= truck_weights[index];
            ++index;
        }
        int tweights = truckQ.front().first;
        int time = truckQ.front().second;
        weight += tweights;
        answer = (time + bridge_length-1);
        cout << answer << " " << index << " " << time <<tweights<< '\n';
        truckQ.pop();

    }

    return answer+1;
}

```


- 이유는 설명하기 어려운데.. q에서 나온 트럭의 시간과 위의 while문에서 한번에 집어넣는 지점에서 겹치는 지점이 있었습니다.
     + 예를 들어서 3초에 트럭이 도착해서 4초에 넣어야하는데 3초 지점에서 while문이 루프를 돌면서 이미 4초에 무언가 넣어서 겹쳤다는 의미..입니다.
- 고치려고 애를 많이 썼는데, 그냥 단순하게 시간을 하나하나 증가시켜서 꼬임을 풀었습니다.  


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <queue>
#include<algorithm>
#include<iostream>
using namespace std;

int solution(int bridge_length, int weight, vector<int> truck_weights) {
    int answer = 1;
    queue<pair<int, int>> truckQ;
    int index = 0;
    while (index < truck_weights.size() || !truckQ.empty())
    {
        if(weight >= truck_weights[index] && index < truck_weights.size())
        {
            truckQ.push(make_pair(truck_weights[index],answer));
            weight -= truck_weights[index];
            ++index;
        }
        int tweights = truckQ.front().first;
        int time = truckQ.front().second;
        int temp = (time + bridge_length - 1);
        //간단하게 트럭이 도착했으면 pop을 합니다.
        if (temp == answer)
        {
            weight += tweights;
            truckQ.pop();
        }
        //cout << answer << " " << index << " " << time <<" "<<tweights<< '\n';
        //시간이 지난다는 의미로 ++answer..
        ++answer;
    }

    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/SQ_03/result.JPG)











 