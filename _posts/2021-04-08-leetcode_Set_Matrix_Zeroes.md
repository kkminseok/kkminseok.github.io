---
title: leetcode(리트코드)73-Set Matrix Zeroes
author: 강민석
date: 2021-04-08 15:45:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 73 - Set Matrix Zeroes 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/set-matrix-zeroes/>  

![](/assets/img/sample/leetcode/73/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/73/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU>에서 추천한 문제입니다. 


-----  

## 4. 문제 해석

- 0으로 들어온 값의 행과 열을 모두 0으로 바꿔버립니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    void fillrow(vector<vector<int>>& matrix,int row,int col){
        for(int i =0;i<col;++i){
            matrix[row][i] =0;
        }
    }
    void fillcol(vector<vector<int>>& matrix,int row,int col){
        for(int i = 0;i<row;++i)
            matrix[i][col]=0;
    }
    
    void setZeroes(vector<vector<int>>& matrix) {
        int rowsize = matrix.size();
        int colsize = matrix[0].size();
        
        bool row[201];
        bool col[201];
        queue<pair<int,int>> q;
        memset(row,false,sizeof(row));
        memset(col,false,sizeof(col));
        //돌면서 0인 값은 queue에 넣음.
        for(int i = 0 ;i<rowsize;++i){
            for(int j = 0; j<colsize; ++j){
                if(matrix[i][j]==0 ){
                    q.push(make_pair(i,j));    
                }
            }
        }
        
        while(!q.empty()){
            int x = q.front().first;
            int y = q.front().second;
            q.pop();
            if(row[x]==false){
                fillrow(matrix,x,colsize);
                row[x]=true;
            }
            if(col[y] == false){
                fillcol(matrix,rowsize,y);
                col[y] =true;
            }
        }
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!


**c++ 16ms 40%**


![](/assets/img/sample/leetcode/73/result.JPG)  








