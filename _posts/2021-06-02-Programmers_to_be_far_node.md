---
title: Programmers_DFS/BFS 가장 먼 노드(python)
author: 강민석
date: 2021-06-02 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -가장 먼 노드 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/49189>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
그래프 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 처음에는 다익스트라로 풀었지만, 시간초과가 나왔습니다.
- 다익스트라는 가중치값을 넣어서 풀었는데 모든 노드의 거리가 1로 같으므로 BFS로 바꿔서 풀었더니 통과하였습니다.




-----  

## 4. 접근 방법을 적용한 코드

```python
from collections import deque
def solution(n, edge):
    # 방문처리겸 거리를 담아놓을 리스트입니다.
    v = [0] * (n+1)
    # 그래프
    graph = [[] for _ in range(n+1)]
    for i in range(len(edge)) : 
        graph[edge[i][0]].append(edge[i][1])
        graph[edge[i][1]].append(edge[i][0])
    #BFS
    dq = deque()
    # 두번째값은 거리입니다.
    dq.append([1,1])
    v[1] = 1
    while dq:
        x,count = dq.popleft()
        for i in range(len(graph[x])) : 
            if v[graph[x][i]] == 0 :
                dq.append([graph[x][i],count+1])
                v[graph[x][i]] = count+1
    #리스트 내에서 가장 큰 값을 찾습니다.               
    maxN = max(v)
    
    #큰 값을 기준으로 갯수를 세줍니다.
    return v.count(maxN)
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
