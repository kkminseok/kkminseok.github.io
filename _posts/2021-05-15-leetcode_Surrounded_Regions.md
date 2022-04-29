---
title: leetcode(리트코드)-130 Surrounded Regions(python)
author: 강민석
date: 2021-05-15 09:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 130 - Surrounded Regions 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/surrounded-regions/> 

![](/assets/img/sample/leetcode/130/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/130/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- board가 주어집니다. 해당 board에서 'X'로 둘러쌓인 'O'를 전부 'X'로 바꿔서 리턴합니다.



-----  

## 5. code  

코드설명
- 경계선과 이어진 'O'를 찾아서 체크해줍니다. 맵 안에서 서로 이어진 'O'들은 어차피 'X'로 바꿔지기 때문입니다.
- 체크된 값은 'O'로 아닌 값은 'X'로 바꿔 정답을 리턴합니다.


**python**

```python
from collections import deque
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        res = board
        row = len(board)
        col = len(board[0])
        v = [0] * row
        for i in range(row):
            v[i] = [0] * col
        
        queue = deque()
        dx = [-1,0,1,0]
        dy = [0,-1,0,1]
        
        for i in range (col) : 
            if board[0][i] == 'O':
                queue.append([0,i])
            if board[row-1][i] =='O':
                queue.append([row-1,i])
        for i in range (row):
            if board[i][0] == 'O' : 
                queue.append([i,0])
            if board[i][col-1] =='O':
                queue.append([i,col-1])
                
        while queue : 
            x,y = queue.popleft()
            v[x][y] = 1
            for i in range(4) :
                newX,newY = x + dx[i], y+dy[i]
                
                if 0<=newX and newX <row and 0<=newY and newY<col and v[newX][newY] == 0 and board[newX][newY] =='O':
                    v[newX][newY] = 1
                    queue.append([newX,newY])
            
        for i in range(row) : 
            for j in range(col):
                if v[i][j] == 0 :
                    res[i][j] = 'X'
                else :
                    res[i][j] = 'O'
        return res     
    
        """
        Do not return anything, modify board in-place instead.
        """
        
```

-----

## 6. 결과 및 후기, 개선점


**python 45% 140ms**

![](/assets/img/sample/leetcode/130/result.JPG)  



