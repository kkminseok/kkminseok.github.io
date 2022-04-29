---
title: leetcode(리트코드)-542 01 Matrix(PYTHON)
author: 강민석
date: 2021-07-29 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 542 - 01 Matrix  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/01-matrix/> 

![](/assets/img/sample/leetcode/542/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/542/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- matrix가 주어집니다.
- 1을 기준으로 가장 가까운 0과의 거리를 찾아 matrix에 저장하고 리턴하세요.



-----  

## 5. code  

코드설명
- 1을 기준으로 찾으면 시간초과가 뜹니다.
- discuss를 참조하여 0을 기준으로 코드를 작성하니 풀리더군요.

**python**

```python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        row = len(mat)
        col = len(mat[0])
        dx = [1,0,-1,0]
        dy = [0,1,0,-1]
        dq = deque()
        for i in range(row):
            for j in range(col):
                if mat[i][j] == 0 :
                    dq.append((i,j))
                else : 
                    mat[i][j] = 987654321
        while dq : 
            x,y = dq.popleft()
            for k in range(4):
                newx,newy = x + dx[k] , y + dy[k]
                z = mat[x][y]+1 
                if 0 <= newx and newx < row and 0 <= newy and newy < col and mat[newx][newy] > z :
                    mat[newx][newy] = z
                    dq.append((newx,newy))
        return mat
    
    
                
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/542/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


