---
title: leetcode(리트코드)54-Spiral Matrix
author: 강민석
date: 2021-04-15 02:45:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 54 - Spiral Matrix 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/spiral-matrix/>  

![](/assets/img/sample/leetcode/54/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/54/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU>에서 추천한 문제입니다. 


-----  

## 4. 문제 해석

- 달팽이모양으로 방문처리를 합니다.


-----  

## 5. code


**c++**

```c++
//좌표 이동을 위한
int dx[4] = {0,1,0,-1};
int dy[4] = {1,0,-1,0};
class Solution {
public:
    
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        //row, col은 나중에도 쓰이므로 미리 변수에 넣어둡니다.
        int row = matrix.size();
        int col = matrix[0].size();
        //방문처리용 벡터
        vector<bool> v(col,false);
        vector<vector<bool>> vc;
        //결과값을 담을 벡터
        vector<int> res;
            
        //방문처리 벡터 초기화
        for(int i = 0 ; i<row;++i)
            vc.push_back(v);

        //BFS를 이용할 것입니다.
        queue<pair<int,int>> q;
        q.push(make_pair(0,0));
        vc[0][0] = true;
        //dis는 방향전환이 필요할 때 사용하는 변수입니다.
        int dis = 0;
        //row * col만큼 돌면 빠져나오기 위한 변수
        int count = 0;
        while(!q.empty()){
            int x = q.front().first;
            int y = q.front().second;
            q.pop();
            res.push_back(matrix[x][y]);
            ++count;
            if(count== row * col) break;
            
            int newX = x + dx[dis];
            int newY = y + dy[dis];
            
            //만약 새 좌표 값이 범위를 벗어나거나 이미 방문한 좌표를 지나려 한다면 좌표를 다시 갱신해줍니다.
            if(newX <0 || newX >= matrix.size() || newY <0 || newY >= matrix[0].size() ||vc[newX][newY]){
              dis = (dis+1)%4;
                newX = x + dx[dis];
                newY = y +dy[dis];
            } 
            if(!vc[newX][newY]){
                q.push(make_pair(newX,newY));
                vc[newX][newY] = true;
            }
        }
        
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!


**c++ 0ms 100%**


![](/assets/img/sample/leetcode/54/result.JPG)  








