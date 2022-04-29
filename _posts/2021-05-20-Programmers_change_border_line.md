---
title: Programmers_2021 Dev Mathcing 행렬 테두리 회전하기 (python)
author: 강민석
date: 2021-05-20 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -행렬 테두리 회전하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/77485#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2021 Dev mathcing 웹 백엔드 개발 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)
- 최악의 경우 1,1,100,100 이 만 번의 쿼리가 반복된다면 brute한 접근으로는 터질거라 생각했습니다.
    + 풀고나니 다행스럽게도(?) 이러한 입력은 없는 것 같습니다.
    + 이러한 문제를 해결하기 위해 1가지 방법을 생각해냈습니다.
- 총 2가지 방법을 생각했습니다.
    + 처음은 메모리를 아끼기 위해 주어진 map에서 모든 것을 처리하여 query에 대한 정보값도 저장하여 효율성을 극대화하려고 했습니다.
    + 이러한 방식은 코드가 너무 복잡할 것 같고, 문제를 해결하기엔 시간을 너무 많이 투자할 것 같았습니다.
    + 2번째 방법은 기존 map에 대한 정보를 담고 있는 copymap을 선언하여, 회전이 끝날 때마다 copymap에 map정보를 갱신해주는 방식입니다.
    + copymap에 회전하지 않은 구간까지 다시 복사하는 것은 시간초과가 나서 회전한 구간만 갱신해주는 코드를 작성하였습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python

import copy
def solution(rows, columns, queries):
    answer = []
    #map
    map = [0]  * rows
    idx = 1
    for i in range(rows) : 
        map[i] = [0] * columns
        for j in range(columns) : 
            map[i][j] =idx
            idx+=1

    #python에서 깊은 복사를 하기 위해서는 밑처럼 써야합니다.
    copymap = copy.deepcopy(map)

    for i in range(len(queries)):
        # 문제에서의 좌표는 1,1로 시작하므로 1씩 빼줍니다.
        x1 = queries[i][0]-1
        y1 = queries[i][1]-1
        x2 = queries[i][2]-1
        y2 = queries[i][3]-1
        #print(x1,y1,x2,y2)
        #step 1
        minN = map[x1][y1]
        for j in range(y1+1,y2+1) : 
            map[x1][j] = copymap[x1][j-1]
            minN = min(minN,map[x1][j])
        #step 2
        for j in range(x1+1,x2+1) : 
            map[j][y2] = copymap[j-1][y2]
            minN = min(minN,map[j][y2])
        #step 3
        for j in range(y2-1,y1-1,-1):
            map[x2][j] = copymap[x2][j+1]
            minN = min(minN,map[x2][j])
        #step 4
        for j in range(x2-1,x1-1,-1):
            map[j][y1] = copymap[j+1][y1]
            minN = min(minN,map[j][y1])

        answer.append(minN)
        
        #copymap 수정
        for j in range(y1,y2+1):
            copymap[x1][j] = map[x1][j]
            copymap[x2][j] = map[x2][j]
        for j in range(x1,x2+1) :
            copymap[j][y2] = map[j][y2]
            copymap[j][y1] = map[j][y1]
        
        #print(map,copymap)
        
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
