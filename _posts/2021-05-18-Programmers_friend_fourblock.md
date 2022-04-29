---
title: Programmers_프렌즈 4블록(python)
author: 강민석
date: 2021-05-18 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -프렌즈 4블록 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17679>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2019 KaKao BLIND RECRUITMENT 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 구현 문제입니다. 문제를 읽어보면 2*2인 경우에만 지워줘야합니다.(4개 이상의 인접한 모든 것이 아님.)
- set을 이용하여 중복 좌표값을 제거하기로 했습니다.
- python에 익숙하지 않아 코드가 많이 더럽습니다. 특히.. 문자열 지우기.





-----  

## 4. 접근 방법을 적용한 코드


```python
from collections import deque
def solution(m, n, board):
    answer = 0
    row = len(board)
    col = len(board[0])
    #dx,dy는 대각선, 오른쪽, 아래를 검사하기 위한 좌표값입니다.
    dx = [0,1,1]
    dy = [1,0,1]
    st = set()
    # check 변수는 단 한번이라도 지워지는 블록이 있는 지 검사하는 변수입니다.
    check =0
    while check == 0 : 
        check = 1
        for i in range(row) : 
            for j in range(col):
                # 공백인 경우 무시
                if board[i][j] != " ":
                    counting  = 1
                    tempst = set()
                    tempst.add((i,j))
                    for k in range(3):
                        newx = i + dx[k]
                        newy = j + dy[k]
                        if 0<= newx and newx < row and 0<=newy and newy < col and board[i][j] == board[newx][newy]: 
                            tempst.add((newx,newy))
                            counting +=1
                        # 하나라도 조건에 만족하지 않으면 빠져 나옵니다.
                        else : 
                            break
                    # for문을 완벽하게 돌았으면 set에 좌표값을 저장합니다.
                    if counting == 4:
                        templist = list(tempst)
                        for k in range(len(templist)):
                            x,y = templist[k]
                            st.add((x,y))
        # 저장된 좌표 값이 있으면 하나씩 빼면서 해당 좌표를 공백으로 만듭니다.
        while st : 
            check = 0
            x,y = st.pop()
            board[x] = board[x][:y] + " " + board[x][y+1:]
            answer += 1
        # board 초기화
        for i in range(row-1,-1,-1) : 
            for j in range(col-1,-1,-1) : 
                if board[i][j] == " ":
                    tempx = i-1
                    #print(i,j,tempx)
                    while tempx >=0  and board[tempx][j] == ' ': 
                        tempx-=1
                    if tempx >=0 :
                        ct = board[tempx][j]
                        board[i] = board[i][:j] + ct + board[i][j+1:]
                        board[tempx] = board[tempx][:j] + " " + board[tempx][j+1:]
                    
                    
                
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
