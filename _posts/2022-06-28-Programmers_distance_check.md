---
title: Programmers_2021 카카오 채용연계형 인턴쉽_거리두기 확인하기
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-06-28 00:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
comments : true
---

**프로그래머스 카카오 채용 - 거리두기 확인하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/81302>

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2021 카카오 채용연계형 인턴쉽 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 그리디하게 풀면 됩니다. 범위가 넓지 않아서 어렵지는 않습니다.
- 코드가 많이 더럽습니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
# 1. 사용자를 다 넣음.
# 2. 서로 거리가 2 이하인 경우만 분리
# 3. 주변을 보고 파티션이 있으면 1 없으면 0
from collections import deque

def getPerson(row,col,places):
    result = deque()
    idx = 0 
    for idx in range(5):
        if(places[row][col][idx]=="P"):
            result.append([col,idx])
    return result

def getDistance(first,second,person_spot):
    judge = abs(person_spot[first][0] - person_spot[second][0]) + abs(person_spot[first][1] - person_spot[second][1])
    return judge

def filterList(person_spot,filter_list):
    size = len(person_spot)
    result = deque()
    for i in range(size):
        for j in range(i+1,size):
            distance = getDistance(i,j,person_spot)
            if distance == 1:
                return [-1]
            elif distance == 2: 
                filter_list.append((person_spot[i],person_spot[j]))
    return filter_list

def judgeDouble(first,second):
    if(first[0] + 2 == second[0]):
        return 1
    elif(first[1] + 2 == second[1]):
        return 2
    return 0

def judgeCross(first,second):
    if(first[0] + 1 ==second[0] and first[1] + 1 == second[1]):
        return 1
    elif(first[0] +1 == second[0] and first[1] - 1 ==second[1]):
        return 2

def solution(places):
    answer = [1] * 5
    row = 0 
    col = 0

    for row in range(5):
        person_spot = deque()
        filter_list = deque()
        for col in range(5):
            person_spot += getPerson(row,col,places)
        judge = filterList(person_spot,filter_list)
        if(len(judge) !=0 and judge[0] == -1):
            answer[row] = 0
            continue
        for idx in range(len(filter_list)):
            first, second = filter_list[idx]
            double_judge = judgeDouble(first,second)
            if(double_judge == 1):
                if (places[row][first[0] + 1][first[1]] != 'X'):
                    answer[row] = 0
                    break
            elif(double_judge==2):
                if(places[row][first[0]][first[1] + 1] != 'X'):
                    answer[row] = 0
                    break
            else:
                cross_judge = judgeCross(first,second)
                if cross_judge == 1:
                    if(places[row][first[0] + 1][first[1]] != 'X' or places[row][first[0]][first[1] + 1] !='X'):
                        answer[row]=0
                        break
                else:
                     if(places[row][first[0]][first[1] - 1] != 'X' or places[row][first[0] + 1][first[1]] !='X'):
                        answer[row]=0
                        break   
    return answer
```


-----



## 5. 결과

![](/assets/img/sample/Programmers/All/kakao_distance_calcu.png)

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.

