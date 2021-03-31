---
title: Programmers_Stack/Queue04 - 프린터
author: 강민석
date: 2021-01-26 15:09:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Queue]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -스택/큐 - 프린터 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/SQ_04/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
스택 큐 Level 2 문제 입니다.    

-----  

## 3. 생각한 것들(문제 접근 방법)

- 원래 같았으면 입력이 크지 않아서 한바퀴씩 돌면서 우선순위를 찾고 해당 우선순위를 만날때까지 큐에 넣고 빼고를 반복하는 코드를 짜려고 했습니다.
- 이러한 반복을 줄이고자 하나의 배열을 더 두었고, 함수를 만들어 최대값을 고려시켰습니다.
- 예를 들면 주어진 vector들을 queue에 넣으면서 우선순위 값들을 임시 배열에 ++연산을 통해 갯수를 세며, 최대값의 인덱스가 전부 소멸되면 그 다음 최댓값을 찾는 함수를 만들었습니다.
- 또한 location이라는 정보를 비교연산 해줘야하므로 index값도 queue에 저장시켜 location이 맞을 때 결과를 도출하도록 만들었습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<iostream>
#include<queue>

using namespace std;

int maxarr[10] = { 0, };
int arrmax = 9;
int promax()
{
    if (maxarr[arrmax] != 0)
    {
        return arrmax;
    }
    else
    {
        while(maxarr[arrmax]==0)
            arrmax--;
        return arrmax;
    }
    
}

int solution(vector<int> priorities, int location) {
    int answer = 0;
    queue<pair<int,int>> q;
    for (size_t i = 0; i < priorities.size(); ++i)
    {
        q.push(make_pair(priorities[i],i));
        maxarr[priorities[i]]++;
    }
    int count = 1;
    while (1)
    {
        int maxnum = promax();
        int pro = q.front().first;
        int index = q.front().second;
        if (pro == maxnum)
        {
            q.pop();
            maxarr[maxnum]--;
            if (index == location)
            {
                answer = count;
                break;
            }
            ++count;
        }
        else
        {
            q.pop();
            q.push(make_pair(pro, index));
        }
    }
    return answer;

}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/SQ_04/result.JPG)











 