---
title: leetcode(리트코드)36-Valid Sudoku
author: 강민석
date: 2021-04-22 11:01:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 36 - Valid Sudoku 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/valid-sudoku/>  

![](/assets/img/sample/leetcode/36/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/36/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 스도쿠 문제입니다. 채울 필요는 없고 스도쿠의 규칙에 맞는 지 확인만 하면 됩니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    bool findrow(vector<char> board){
        bool checknum[10]={0,};
        for(int i = 0 ; i < 9;++i){
            if(board[i] =='.')continue;
            if(checknum[board[i]-'0']) return false;
            checknum[board[i]-'0']= true;
        }
        return true;
    }
    bool findcol(int j,vector<vector<char>> b){
        bool checknum[10]={0,};
        for(int i = 0 ; i < 9;++i){
            if(b[i][j] =='.')continue;
            if(checknum[b[i][j]-'0']) return false;
            checknum[b[i][j]-'0'] = true;
        }
        return true;
    }
    
    bool subboxc(int i ,int j , vector<vector<char>> b){
        bool checknum[10]={false,};
        for(int row = i ; row< i+3; ++row){
            for(int col = j ; col < j+3 ; ++col){
                if(b[row][col] == '.') continue;
                if(checknum[b[row][col] -'0']) return false;
                checknum[b[row][col] -'0'] =true;
            }
        }
        return true;
    }
    
    bool isValidSudoku(vector<vector<char>>& board) {
        //row 검사
        int row = board.size();
        int col = board[0].size();
        for(int i = 0 ; i <row;++i){
            if(!findrow(board[i])) return false;
        }
        //col 검사
        for(int j = 0 ; j <col;++j){
            if(!findcol(j,board)) return false;
        }
        //sub box
        for(int i = 0 ; i < 9; i += 3){
            for(int j = 0 ; j <9; j+=3){
                if(!subboxc(i,j,board)) return false;
            }
        }
        
        return true;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 47% python ??%** 
![](/assets/img/sample/leetcode/36/result.JPG)  






