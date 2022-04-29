---
title: Programmers_2017 카카오코드 예선 카카오프렌즈 컬러링북(c++)
author: 강민석
date: 2021-08-27 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 2017 카카오코드 예선 카카오프렌즈 컬러링북 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/1829>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2017카카오코드 예선 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- BFS나 DFS를 묻는 문제입니다. 구현방식만 알면 어렵지 않습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <vector>
//BFS를 사용하기 위함.
#include <queue>
//max()함수를 사용하기위함.
#include <algorithm>
//입출력
#include <iostream>
using namespace std;

//좌표이동
int dx[4] = {0,0,1,-1};
int dy[4] = {1,-1,0,0};

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
vector<int> solution(int m, int n, vector<vector<int>> picture) {
    int number_of_area = 0;
    int max_size_of_one_area = 0;
    
    vector<int> answer(2);
    answer[0] = number_of_area;
    answer[1] = max_size_of_one_area;
    
    int row = picture.size();
    int col = picture[0].size();
    for(int i = 0; i < row; ++i){
        for(int j = 0; j < col; ++j){
            if(picture[i][j] != 0){
                int res = 0;
                answer[0] += 1;
                int pr = picture[i][j];
                //BFS
                queue<pair<int,int>> q;
                q.push(make_pair(i,j));
                //방문처리는 0으로 바꿔주는걸로 합니다. 효율성 고려
                while(!q.empty()){
                    int x = q.front().first;
                    int y = q.front().second;
                    q.pop();
                    for(int k = 0 ; k < 4; ++k){
                        int newx = x + dx[k];
                        int newy = y + dy[k];
                        if(0 <= newx and newx < row and 0 <= newy and newy < col and pr == picture[newx][newy]){
                            res += 1;
                            q.push(make_pair(newx,newy));
                            picture[newx][newy] = 0;
                        }
                    }
                }
                answer[1] = max(answer[1],res);
                
            }
        }
    }
    
    return answer;
}
```


-----



## 5. 결과

필요시. python 짜드리겠습니다.
설명이 필요시 댓글달아주세요.















 
