---
title: Programmers_게임 맵 최단거리
author: 강민석
date: 2021-05-07 05:05:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -게임 맵 최단거리 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/1844?language=cpp>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
찾아라 프로그래밍 마에스터 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- BFS나 DFS로 풀면 되는데, python으로는 해결이 안되어서.. 왜인지 모르게 그래서 c++로 풀었습니다.






-----  

## 4. 접근 방법을 적용한 코드


```c++
#include<vector>
#include<queue>
#include<algorithm>
using namespace std;

int dx[4] = {0,1,0,-1};
int dy[4] = {1,0,-1,0};

int solution(vector<vector<int> > maps)
{
    int answer = 1234567;
    int row = maps.size();
    int col = maps[0].size();
    bool v[101][101] = {false,};
    
    queue<pair<pair<int,int>,int>> q;
    q.push(make_pair(make_pair(0,0),1));
    v[0][0] = true;
    while(!q.empty()){
        int x = q.front().first.first;
        int y = q.front().first.second;
        int count = q.front().second;
        if(x == row-1 && y ==col-1)
            answer = min(answer,count);
            
        q.pop();
        
        for(int k = 0 ; k <4;++k){
            int newX = x + dx[k];
            int newY = y + dy[k];
            if(0<=newX && newX <row && 0<=newY && newY<col && !v[newX][newY] && maps[newX][newY]==1){
                v[newX][newY] =true;
                q.push(make_pair(make_pair(newX,newY),count+1));
            }
        }
            
    }
    
    if(answer != 1234567)
        return answer;
    else
        return -1;
}
```

**python(안되는 코드 로직은 같은데 why?..)**

```python
from collections import deque

dx = [0,1,0,-1];
dy = [1,0,-1,0];

def solution(maps):
    answer = 12345678
    q = deque()
    q.appendleft([0,0,1])
    
    row = len(maps)
    col = len(maps[0])
    v = [0] * 101
    for i in range(101):
        v[i] = [0]*101
    v[0][0]=1    
    while q:
        x,y,count = q.popleft()
        if x == row-1  and y == col-1 :
            answer = min(answer,count)
        for i in range (len(dx)):
            newX = x + dx[i]
            newY = y +dy[i]
            if 0<= newX and newX<row and 0<= newY and newY < col and v[newX][newY] != 1 and maps[newX][newY] == 1 :
                v[newX][newY] = 1
                count += 1
                q.append([newX,newY,count])
    
    if answer != 12345678 : 
        return answer
    else :
        return -1
```


-----



## 5. 결과

python코드 왜 안되는 지 아시는 분?..















 
