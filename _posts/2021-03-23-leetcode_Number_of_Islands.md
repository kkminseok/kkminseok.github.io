---
title: leetcode(리트코드)200-Number of Islands
author: 강민석
date: 2021-03-23 05:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 200 - Number of Islands 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-islands/>  

![](/assets/img/sample/leetcode/200/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/200/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 어려울게 없는 BFS,DFS문제입니다. 1로 이루어진 섬의 개수를 셉니다.


-----  

## 5. code


**c++**

```c++
class Solution {
    bool v[301][301]={false,};
    int dx[4] ={0,-1,1,0};
    int dy[4] = {1,0,0,-1};
public:
    int BFS(int i,int j,int row,int col,vector<vector<char>>& grid)
    {
        queue<pair<int,int>> q;
        q.push(make_pair(i,j));
        v[i][j]=true;
        
        while(!q.empty())
        {
            int x = q.front().first;
            int y = q.front().second;
            q.pop();
            for(int k =0;k<4;++k)
            {
                int newX = x+dx[k];
                int newY = y+dy[k];
                if(0<=newX && newX<row && 0<=newY && newY<col && !v[newX][newY] && grid[newX][newY]=='1')
                {
                    v[newX][newY] = true;
                    q.push(make_pair(newX,newY));
                }
            }
        }
        return 1;
    }
    int numIslands(vector<vector<char>>& grid) {
        int result = 0;
        memset(v,false,sizeof(v));
        int row = grid.size();
        int col = grid[0].size();
        for(size_t i =0;i<row;++i)
        {
            for(size_t j = 0 ;j<col;++j)
                if(grid[i][j] == '1'&&!v[i][j])
                    result += BFS(i,j,row,col,grid);
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 80% python ??%** 
![](/assets/img/sample/leetcode/200/result.JPG)  

