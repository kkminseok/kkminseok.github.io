---
title: Programmers_월간 코드 챌린지 시즌1 삼격 달팽이(python)
author: 강민석
date: 2021-05-21 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -삼각 달팽이 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/68645>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌1 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 2차원 list를 만들어 map을 구현하였습니다.
- 첫 번째로 row의 좌표를 내리면서 값을 저장하였습니다.
- 두 번째로 col을 좌표를 올려가면서 값을 저장하였습니다.
- 세 번째로 row와 col을 1씩 줄이면서 값을 저장하였습니다.
- 위 세 방법을 max값인 n(n+1)/2 까지 반복하면 됩니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(n):
    answer = []
    map = []
    for i in range(n) :
        map.append([])
        map[i] = [0] * (i+1)
    nowx = -1
    nowy = 0
    max = (n *(n+1))//2
    idx = 1 
    while idx <= max :
        while nowx+1 < n and map[nowx+1][nowy] ==0 :
            map[nowx+1][nowy] = idx
            nowx+=1
            idx+=1
        while nowy+1 <n and map[nowx][nowy+1]==0:
            map[nowx][nowy+1] = idx
            nowy+=1
            idx+=1
        while nowx-1 >0 and nowy-1 >0 and map[nowx-1][nowy-1] == 0:
            map[nowx-1][nowy-1] = idx
            nowx-=1
            nowy-=1
            idx+=1
        
    for i in range(n):
        for j in range(len(map[i])):
            answer.append(map[i][j])
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
