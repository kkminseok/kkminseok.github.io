---
title: leetcode(리트코드)3월25일 challenge417-Pacific Atlantic Water Flow
author: 강민석
date: 2021-03-26 00:00:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 25일 - Pacific Atlantic Water Flow 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/pacific-atlantic-water-flow/>  

![](/assets/img/sample/leetcode/417/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/417/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 25일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- Pacific(~) 와 Atlantic(*)로 둘다 갈 수 있는 물결이면 해당 좌표를 리턴합니다.
- 좌표에 써있는 값은 높이로, 작거나 같은 물결로 이동할 수 있습니다.
- ~나 *에 닿으면 이동한것으로 칩니다.
- 저는 해석을 처음에 잘 못해서 좋은 코드가 나오지 못했습니다.
- Pacific와 Atlantic을 가는 경우를 한 번에 찾아줬는데, 하나만 찾고 찾지 못했으면 그냥 제외시키는 등 로직을 구성하면 훨씬 빠른 코드가 될 것입니다. 




-----  

## 5. code

**c++**

```c++

int dx[4] = {0,1,0,-1};
int dy[4] = {1,0,-1,0};
bool v[151][151]={false,};
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
     //BFS
        if(matrix.size()==0)
            return matrix;
        int row = matrix.size();
        int col = matrix[0].size();
        vector<vector<int>> result;
        
        for(int i =0 ; i < row;++i)
        {
            for(int j = 0;j<col;++j)
            {
                bool pc = false;
                bool ac = false;
                //BFS
                memset(v,false,sizeof(v));
                //Pacific
                queue<pair<int,int>> pacific;
                pacific.push(make_pair(i,j));
                while(!pacific.empty())
                {
                    int x = pacific.front().first;
                    int y = pacific.front().second;
                    //cout<<"x"<<x<<"y"<<y<<'\n';
                    if(x==0||y==0)
                    {
                        pc=true;
                    }
                    if(x==row-1 || y==col-1)
                    {
                        ac=true;
                    }
                    if(pc && ac)
                    {
                        //cout<<"pc  ac\n";
                        vector<int> temp;
                        temp.push_back(i);
                        temp.push_back(j);
                        result.push_back(temp);
                        break;
                    }
                    pacific.pop();
                    for(int k = 0;k<4;++k)
                    {
                        int newX = x + dx[k];
                        int newY = y + dy[k];
                        if(0<=newX && newX<row && 0<=newY && newY<col && matrix[newX][newY]<=matrix[x][y] && !v[newX][newY])
                        {
                            v[newX][newY]=true;
                            pacific.push(make_pair(newX,newY));
                        }
                    }
                }
            }
        }
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 5%**
![](/assets/img/sample/leetcode/417/result.JPG)  

