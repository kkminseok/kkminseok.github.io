---
title: leetcode(리트코드)64-Minimum Path Sum
author: 강민석
date: 2021-03-10 01:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 64 - Minimum Path Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-path-sum/>  

![](/assets/img/sample/leetcode/64/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/64/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 숫자가 들어있는 2차원 벡터가 들어옵니다. 왼쪽 맨 위에서 오른쪽 맨 아래에 도달할 때 숫자를 더합니다. 숫자가 최소가 되도록 경로를 짰을 때 숫자를 리턴하세요.



-----  

## 5. code

**python**

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        row = len(grid)
        col = len(grid[0])
        for i in range(1,row):
            grid[i][0] += grid[i-1][0]
        for j in range(1,col):
            grid[0][j] += grid[0][j-1]
        for i in range(1,row) :
            for j in range(1,col):
                grid[i][j] += min(grid[i-1][j],grid[i][j-1])
        return grid[row-1][col-1]
```        

-----  

**c++**

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int row = grid.size();
        int col = grid[0].size();
        for(size_t i = 1;i<row; ++i)
            grid[i][0] += grid[i-1][0];
        for(size_t j = 1; j<col; ++j)
            grid[0][j] += grid[0][j-1];
        
        int i = 1;
        int j = 1;
        for(; i<row;++i)
        {
            for(j=1; j<col; ++j)
                grid[i][j] += (min(grid[i-1][j], grid[i][j-1]));
        }
        
        return grid[row-1][col-1];
    }
};
```

-----

## 6. 결과 및 후기, 개선점

python 92%, c++ 96%

![](/assets/img/sample/leetcode/64/result.JPG)  

