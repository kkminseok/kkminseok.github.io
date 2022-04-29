---
title: leetcode(리트코드)5월12일 challenge304-Range Sum Query 2D Immutable
date: 2021-05-12 10:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 12일 - Range Sun Query 2D Immutable 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/range-sum-query-2d-immutable/>  

![](/assets/img/sample/leetcode/304/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/304/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 12일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 주어진 matrix에서 row1 ~ row2, col1 ~ col2 까지의 합을 구하는 것입니다.
- brute하게 풀면 시간초과가 떠서 다른 방법으로 풀어야합니다.
- Discuss에서 잘 정리된 설명이 있습니다.  
 <https://leetcode.com/problems/range-sum-query-2d-immutable/discuss/75350/Clean-C%2B%2B-Solution-and-Explaination-O(mn)-space-with-O(1)-time>





-----  

## 5. code


**c++**

```c++
class NumMatrix {
public:
    int row,col;
    vector<vector<int>> sums;
    
    NumMatrix(vector<vector<int>>& matrix) {
        row = matrix.size();
        col = matrix[0].size();
        
        sums = vector<vector<int>>(row+1,vector<int>(col+1,0));
        for(int i =1 ; i <=row;++i){
            for(int j =1 ; j<=col;++j){
                sums[i][j] = matrix[i-1][j-1] + sums[i-1][j] + sums[i][j-1] - sums[i-1][j-1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return sums[row2+1][col2+1] - sums[row2+1][col1] - sums[row1][col2+1] + sums[row1][col1];
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */

```

-----

## 6. 결과 및 후기, 개선점

나중에 다시 풀 것.




