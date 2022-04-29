---
title: leetcode(리트코드)3월19일 challenge841-Keys and Rooms
author: 강민석
date: 2021-03-19 12:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 19일 - Key and Rooms 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/keys-and-rooms/>  

![](/assets/img/sample/leetcode/841/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/841/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 19일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 2차원 벡터 rooms이 주어집니다.
- rooms[i][j]에 rooms[i]를 열 수 있는 key가 있습니다.
- 모든 문을 열 수 있으면 true 없으면 false를 리턴합니다.
- BFS나 DFS로 풀면 됩니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        bool check[3001]={false,};
        check[0]=true;
        //bfs
        queue<int> q;
        q.push(0);
        while(!q.empty())
        {
            int x = q.front();
            q.pop();
            for(int k = 0;k<rooms[x].size();++k)
            {
                if(!check[rooms[x][k]])
                {
                    check[rooms[x][k]]=true;
                    q.push(rooms[x][k]);
                }
            }
        }
        for(size_t i =0;i<rooms.size();++i)
        {
            if(!check[i])
                return false;
        }
        return true;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 99%**
![](/assets/img/sample/leetcode/841/result.JPG)  

