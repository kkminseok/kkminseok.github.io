---
title: leetcode(리트코드)-1905 Count Sub Islands(PYTHON)
author: 강민석
date: 2021-07-18 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1905 - Count Sub Islands  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-sub-islands/> 

![](/assets/img/sample/leetcode/1905/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1905/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다. 


-----  

## 4. 문제 해석

- grid1과 grid2가 주어집니다.
- grid2의 인접한 섬들이 모두 grid1에 종속될 때 count를 하나 증가시킵니다.
- 최종 count값을 리턴합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
        row = len(grid2)
        col = len(grid2[0])
        res = 0
        dx =[-1,0,1,0]
        dy = [0,-1,0,1]
        v = [0] * row
        for i in range(row) : 
            v[i] = [0] * col
        for i in range(row) : 
            for j in range(col):
                if v[i][j] == 0 and grid2[i][j] == 1 :
                    if grid1[i][j] == 0 :
                        continue
                    else : 
                        dq = deque()
                        dq.append((i,j))
                        v[i][j] = 1
                        check = False
                        while dq : 
                            x,y = dq.popleft()
                            for k in range(4) : 
                                newx,newy = x + dx[k] , y + dy[k]
                                if 0<= newx and newx <row and 0 <= newy and newy < col and grid2[newx][newy] == 1:
                                    if v[newx][newy] == 1 :
                                        continue
                                    else : 
                                        if grid1[newx][newy] == 0 : 
                                            check = True
                                        dq.append((newx,newy))
                                        v[newx][newy] = 1    
                        if check == False : 
                            res+=1
        return res
                  
                                        
                        
                        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1905/result.JPG)  

필요시 c++로 작성해드립니다.

모르겟으면 댓글 부탁드립니다.

