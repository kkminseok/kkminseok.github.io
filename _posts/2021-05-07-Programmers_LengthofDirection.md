---
title: Programmers_방문 길이
author: 강민석
date: 2021-05-07 12:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -방문 길이 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/49994>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- BFS로 풀면 간단하게 풀것이라 생각했지만, 위에서 들어오는 경우, 밑에서 들어오는 경우가 각각 다른경우로 계산해야하기 때문에 그 부분에서 애먹었습니다.
- 다른 사람들의 코드를 참고하여 4차원 배열을 썼는데 [fromx][fromy][tox][toy] 로 이해하시면 됩니다.





-----  

## 4. 접근 방법을 적용한 코드


```c++
#include <string>
#include <queue>
#include <iostream>
using namespace std;

int findx(char dir){
    if(dir =='U')
        return -1;
    else if(dir =='L' || dir =='R')
        return 0;
    else
        return 1;
}
int findy(char dir){
    if(dir =='U' || dir =='D')
        return 0;
    else if(dir == 'L')
        return -1;
    else
        return 1;
}

int solution(string dirs) {
    bool map[11][11][11][11]={0,};
    int startx = 5,starty = 5;
    int idx = 0 ;
    int answer = 0;
    queue<pair<int,int>> q;
    q.push(make_pair(startx,starty));
    while(idx < dirs.size()){
        int x = q.front().first;
        int y = q.front().second;

        q.pop();

        int newX = x + findx(dirs[idx]);
        int newY = y + findy(dirs[idx]);
        if(0<=newX  && newX <11 && 0<=newY && newY<11 ){
            if(!map[x][y][newX][newY]){
                map[x][y][newX][newY] = true;
                map[newX][newY][x][y] = true;
                ++answer;
            }
            q.push(make_pair(newX,newY));
        }
        else
            q.push(make_pair(x,y));
        ++idx;
    }
    return answer;
}
```


-----



## 5. 결과

필요시. python로 짜드리겠습니다.















 
