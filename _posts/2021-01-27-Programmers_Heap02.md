---
title: Programmers_Heap02 - 디스크 컨트롤러
author: 강민석
date: 2021-01-27 12:09:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Heap]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 디스크 컨트롤러 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/HEAP_02/Problem.JPG)  
![](/assets/img/sample/Programmers/HEAP_02/Problem2.JPG)  
![](/assets/img/sample/Programmers/HEAP_02/Problem3.JPG)  



-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 테스트 케이스 4,7,8,9,18이 계속 틀렸습니다.
- 요청시간이 온 순서대로 처리해야하기에 요청시간을 기준으로 정렬을 해줘야한다고 생각했습니다.
- 만약 요청 작업 중에 여러 작업의 요청이 들어오면 가장 빨리 끝낼 수 있는 것이 우선순위를 갖고, 가장 빨리 끝낼 수 있는 것 마저 같다면 요청이 먼저 온 것을 처리합니다.
- 작업은 단일 작업으로 해야합니다. 요청이 언제 올 지 모르기 때문입니다. -> 이것 때문에 틀림.

**틀린 코드입니다**
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>
#include<cmath>
#include<algorithm>
using namespace std;

bool pred(pair<int, int> a, pair<int, int> b)
{
    if (a.second == b.second)
        return a.first < b.first;
    return a.second > b.second;
}

int solution(vector<vector<int>> jobs) {
    int answer = 0;
    int size = jobs.size();
    int ms = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> tempq;


    //요청 시간 순으로 정렬하면서 큐에 넣습니다.
    for (size_t i = 0; i < jobs.size(); ++i)
    {
        q.push(make_pair(jobs[i][0], jobs[i][1]));
    }
    while (!q.empty())
    {
        //요청시간이 짧다면
        if (q.top().first <= ms)
        {
            while (!q.empty() && q.top().first <= ms)
            {
                int request = q.top().first;
                int time = q.top().second;
                tempq.push(make_pair(time, request));
                q.pop();
            }
            if (!tempq.empty())
            {
                int request = tempq.top().second;
                int time = tempq.top().first;
                tempq.pop();
                ms += time;
                answer += ms - request;
            }
        }
        else
        {
            //요청시간안에 들어오지 않았으면 ms를 증가시킵니다.
            ms++;
        }
    }
    while (!tempq.empty())
    {
        int request = tempq.top().second;
        int time = tempq.top().first;
        tempq.pop();
        ms += time;
        answer += ms - request;
    }

    answer = trunc(answer / size);
    return answer;
}
```


**고친 점**

- 구글링을 하다보니 2차워 벡터도 sort()함수를 통해 첫 번째 요소로 정렬이 가능하다는 점을 알았습니다.
- 다른 사람들의 질문을 보며 힌트를 얻고, 단일 처리코드를 제외한 다른 것들의 반복적인 코드를 제거했습니다.
- 위의 로직과 다른 점이 기존 배열에 직접 참조하느냐, 우선순위 큐에 넣은 값을 참조하느냐 차이인 것 같은데 왜 틀렸는 지 모르겠습니다..

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>
#include<cmath>
#include<algorithm>
using namespace std;



int solution(vector<vector<int>> jobs) {
    int answer = 0;
    sort(jobs.begin(), jobs.end());
    int ms = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    int i = 0;
    while (i<jobs.size() || !pq.empty())
    {
        while (i<jobs.size() && jobs[i][0] <= ms)
        {
            pq.push(make_pair(jobs[i][1], jobs[i][0]));
            ++i;
        }

        //작업 처리 하나만
        if (!pq.empty())
        {
            int time = pq.top().first;
            int request = pq.top().second;
            pq.pop();
            ms += time;
            answer += (ms - request);
        }
        else
        {
            ms = jobs[i][0];
        }
        
    }
  
    answer = trunc(answer / jobs.size());
    return answer;
}
```
-----

## 5. 결과

운영체제 과목에서 이 로직을 c++로 짰었습니다.  
코드가 너무 길어서 참고는 안했습니다.  

![](/assets/img/sample/Programmers/HEAP_02/result.JPG)











 