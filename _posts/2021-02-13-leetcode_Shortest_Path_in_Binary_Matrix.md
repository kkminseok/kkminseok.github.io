---
title: leetcode(리트코드)2월13일 challenge1091-Shortest Path in Binary Matrix
author: 강민석
date: 2021-02-13 17:30:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge13 - Shortest path in Binary Matrix 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/shortest-path-in-binary-matrix>  

![](/assets/img/sample/leetcode/1091/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1091/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월13일자 챌린지 문제입니다.   

-----  

## 4. 문제 해석

- 어렵지 않은 일반적인 BFS 문제입니다. 대각선으로 움직일 수 있다는 것이 특이점입니다. 


-----  

## 5. code
```c++
const int MAX = 101;

bool v[MAX][MAX]={false};
int MAP[MAX][MAX];

int dx[8]={-1,1,1,-1,-1,0,1,0};
int dy[8]={1,1,-1,-1,0,1,0,-1};

class Solution {
public:
    int BFS(vector<vector<int>>& grid,int _size)
    {
        memset(v,false,sizeof(v));
        int size = _size;
        //x y count
        queue<pair<pair<int,int>,int>> q;
        q.push(make_pair(make_pair(0,0),1));
        v[0][0]=true;
        while(!q.empty())
        {
		    int x = q.front().first.first;
		    int y = q.front().first.second;
		    int count = q.front().second;
            if(x == (size-1) && y ==(size-1))
                return count;
            q.pop();
            for(int k=0;k<8;++k)
            {
                int newX = x + dx[k];
                int newY = y + dy[k];

                if(0<=newX && newX<size && 0<=newY && newY<size && grid[newX][newY]==0 && !v[newX][newY])
                {
                    q.push(make_pair(make_pair(newX,newY),count+1));
                    v[newX][newY]=true;
                }
            }
        }
        return -1;
    }
    
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        if(grid[0][0]==1)
            return -1;
        int result =BFS(grid,grid.size());
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점
  
**시간 93%**
![](/assets/img/sample/leetcode/1091/result.JPG) 


