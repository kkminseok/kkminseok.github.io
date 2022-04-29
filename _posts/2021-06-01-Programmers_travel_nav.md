---
title: Programmers_DFS/BFS 여행경로(python)
author: 강민석
date: 2021-06-01 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -여행경로 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/43164#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
깊이/너비 우선 탐색 DFS/BFS 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 백트래킹 방법을 사용하지 않으면 시간초과가 뜹니다.
- python에서 재귀 동작방식에 대한 이해가 부족하여 결과값들을 다 넣고 앞부분만 잘랐습니다. 개선이 필요합니다.






-----  

## 4. 접근 방법을 적용한 코드

```python
from collections import deque
def DFS(templi,dic,start,finish,v,answer):
    #결과값은 항상 주어진 tickets 리스트의 크기 +1입니다.
    if len(templi) == finish : 
        # 결과값을 다 넣어버립니다.
        for i in range(len(templi)):
            answer.append(templi[i])
        return templi
    #dictionary에 값이 있을 때
    if start in dic : 
        for i in range(len(dic[start])) :
            # 방문 처리가 되지 않은 것들을 돕니다.
            if v[start][i]==0 : 
                #templi에 추가
                templi.append(dic[start][i])
                #방문처리
                v[start][i] = 1
                # 재귀
                DFS(templi,dic,dic[start][i],finish,v,answer)
                #맨 뒤의 값을 제거 및 방문처리 제거합니다.
                templi.pop()
                v[start][i] = 0
        return templi

    
def solution(tickets):
    answer = []
    dic = {}
    v = {}
    finish = len(tickets)+1
    #dic에 dic[출발지] = 도착지 값들을 deque형태로 저장합니다. 방문처리를 위한 v도 마찬가지로 저장합니다. 0은 방문하지 않은 것 1은 방문한 것입니다.
    for i in range(len(tickets)):
        start = tickets[i][0]
        end = tickets[i][1]
        if start not in dic:
            dic[start] = deque()
            v[start] = deque()
            dic[start].append(end)
            v[start].append(0)
        else : 
            dic[start].append(end)
            v[start].append(0)
    #정렬
    for i in dic : 
        dic[i] = sorted(dic[i])
    DFS(["ICN"],dic,"ICN",finish,v,answer)
    # 결과값들을 다 넣었으므로 앞부분을 자릅니다. 이미 정렬을 했기에 앞부분이 정답일 수 밖에 없습니다.
    return answer[:finish]
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
