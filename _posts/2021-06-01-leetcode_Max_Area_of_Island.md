---
title: leetcode(리트코드)6월01일 challenge695-Max Area of Island(python)
date: 2021-06-01 04:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 01일 - Max Area of Island 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/max-area-of-island/>  

![](/assets/img/sample/leetcode/695/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/695/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 01일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- Island라는 맵이 주어집니다.
- 1로 이어진 부분은 Island의 영역으로 각기 다른 Island의 넓이중 가장 큰 값을 찾아 리턴하세요.






-----  

## 5. code


**python**

```python

#좌표 이동 값
dx=[-1,0,1,0]
dy=[0,1,0,-1]
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        def BFS(i,j,row,col,grid,v):
            area = 1
            dq = deque()
            dq.append([i,j])
            v[i][j] = 1
            while dq : 
                x,y = dq.popleft()
                for k in range(4):
                    newx,newy = x+dx[k], y+dy[k]
                    if 0<=newx and newx<row and 0<=newy and newy<col and grid[newx][newy] == 1 and v[newx][newy] == 0 :
                        v[newx][newy] = 1
                        dq.append([newx,newy])
                        area+=1
            return area
        row = len(grid)
        ans = 0
        col = len(grid[0])
        v = [0] * row
        for i in range(row) : 
            v[i] = [0] * col
        
        for i in range(row) : 
            for j in range(col):
                if grid[i][j] == 1 and v[i][j] ==0:
                    ans = max(ans,BFS(i,j,row,col,grid,v))
        return ans
            
        
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/695/result.JPG)  

필요시 c++로 풀어드립니다.



