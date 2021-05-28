---
title: Programmers_연습문제 가장 큰 정사각형 찾기(python)
author: 강민석
date: 2021-05-28 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -가장 큰 정사각형 찾기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12905>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 재귀함수로 풀어야한다는 생각에 접근하지 못했습니다.
- DP로 풀 수 있습니다.
- 대각선, 바로 위의 열, 옆의 열을 검사하여 값을 갱신하면 됩니다.




-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(board):
    row = len(board)
    col = len(board[0])
    
    DP = [0] * row
    size = 0
    for i in range(row):
        DP[i] = [0] * col
    for i in range(row) : 
        for j in range(col) : 
            if i ==0 or j== 0 or board[i][j] == 0 : 
                DP[i][j] = board[i][j]
            else : 
                DP[i][j] = min(DP[i-1][j-1],min(DP[i-1][j],DP[i][j-1])) + 1
            size = max(size,DP[i][j])

    return size*size
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
