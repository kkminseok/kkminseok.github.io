---
title: Programmers_위클리 챌린지 최소직사각형
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-06-22 00:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
  path: 
comments : true
---
**프로그래머스 위클리 챌린지 - 최소직사각형 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/86491?language=python3>

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
프로그래머스 위클리 챌린지 문제입니다.

Level 1난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 그리디하게 풀면 됩니다.
- 하나씩 늘려가면 직사각형의 넓이가 어떻게 될 지 생각하면 접근하기 쉽습니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(sizes):
    pointx, pointy = sizes[0]
    x = max(pointx,pointy)
    y = min(pointx,pointy)
    for i in range(1,len(sizes)):
        newx = max(sizes[i][0], sizes[i][1])
        newy = min(sizes[i][0], sizes[i][1])
        x = max(x,newx)
        y = max(y,newy)
    return x*y
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.

