---
title: Programmers_위클리 챌린지 교점에 별 만들기
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-06-29 00:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
  path: 
comments : true
---


**프로그래머스 위클리 챌린지 - 교점에 별 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/87377#qna>

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
위클리 챌린지 문제 입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 구현이 좀 복잡합니다. 좌표계에서 굉장히 헷갈리는데, 교점 이동을 잘 해준다면 어렵지 않게 푸실 수 있을겁니다.
- 그리고 초기에 min,max값을 아예 높게 설정하지 않으면 28테케,29테케에서 에러가 납니다..왠지는 모름.


-----  

## 4. 접근 방법을 적용한 코드

```python
from collections import deque

def solution(line):
    INF = float('inf')
    size = len(line)
    result = []
    max_x,max_y,min_x,min_y = -INF,-INF,INF,INF
    st = set()
    for i in range(size):
        for j in range(i+1,size):
            A,B,E = line[i]
            C,D,F = line[j]
            low = A*D - B*C
            if low == 0:
                continue
            x = (B*F - E*D) / low
            y = (E*C - A*F) / low
            if x - int(x) or y - int(y):
                continue
            x = int(x)
            y = int(y)
            max_x = max(max_x,x)
            min_x = min(min_x,x)
            max_y = max(max_y,y)
            min_y = min(min_y,y)
            st.add((x,y))
        result= list(st)
    # makeMap
    width = max_x - min_x + 1
    height = max_y - min_y + 1
    map = [['.']*width for i in range(height)]
    
    for x,y in result:
        map[max_y-y][x-min_x] = "*"
    
    return [''.join(s) for s in map]
```


-----



## 5. 결과

![](/assets/img/sample/Programmers/All/make_star.png)

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.

