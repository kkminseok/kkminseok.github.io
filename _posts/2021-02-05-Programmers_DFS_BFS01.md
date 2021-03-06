---
title: Programmers_BFS/DFS01 - 타겟 넘버
author: 강민석
date: 2021-02-05 12:03:00 +0800
categories: [Algorithm,DFS]
tags: [Programmers,DFS]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 타겟 넘버 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/BDFS01/Problem.JPG)  



-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 방법에 대해서 끝까지 연산을 해서 확인해야하므로 DFS가 적합하다고 생각하고 풀었습니다.  


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;
int result = 0;
void DFS(vector<int> numbers, int target,int count, int sum)
{
    if (count == numbers.size())
    {
        if(target==sum)
            result++;
        return;
    }
    DFS(numbers, target, count + 1, sum + numbers[count]);
    DFS(numbers, target, count + 1, sum - numbers[count]);
}


int solution(vector<int> numbers, int target) {
    int answer = 0;
    DFS(numbers, target,0,0);
    answer = result;
    cout << answer;
    return answer;
}

```
-----

## 5. 결과

![](/assets/img/sample/Programmers/BDFS01/result.JPG)  













 
