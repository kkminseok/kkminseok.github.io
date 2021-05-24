---
title: Programmers_Summer/Winter Coding(~2018) 배달(python)
author: 강민석
date: 2021-05-24 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -배달 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12978>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer/Winter Coding(~2018) 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 다익스트라 알고리즘 문제입니다.
- 마을에 대한 거리값이 중복해서 들어올 경우가 있습니다. 이 경우를 해결해주면 어렵지 않습니다.

-----  

## 4. 접근 방법을 적용한 코드


```python

from queue import PriorityQueue
def solution(N, road, K):
    answer = 0
    res = [987654321] * (N+1)
    map = [0] * (N+1)
    que = PriorityQueue()
    for i in range(N+1):
        map[i] = [0] * (N+1)
    for i in range(len(road)):
        row,col,dis = road[i][0],road[i][1],road[i][2]
        if map[row][col] != 0 :
            if map[row][col] > dis :
                map[row][col] = dis
                map[col][row] = dis
        else:
            map[row][col] = dis
            map[col][row] = dis
    res[1] = 0
    que.put([1,0])
    while not que.empty() :
        curr,distance = que.get()
        distance = -distance
        if res[curr] < distance : 
            continue
        for i in range(len(map[curr])):
            if map[curr][i] != 0 :
                next = i
                nextdis = map[curr][i] + distance
                if nextdis < res[next] :
                    res[next]= nextdis
                    que.put([next,-nextdis])
    
    for k in range(1,len(res)):
        if res[k] <=K:
            answer+=1
    print(res)

    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
