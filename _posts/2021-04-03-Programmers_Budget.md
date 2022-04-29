---
title: Programmers_Summer/Winter Coding(~2018) - 예산
author: 강민석
date: 2021-04-03 00:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 예산 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12982>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 예산이 주어지고 요청 예산벡터가 주어집니다.
- 최대한 많은 예산을 줄 수 있는 경우의 갯수를 리턴합니다.
- 작은 값을 순서대로 넣으면 되므로 어렵지 않습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <iostream>
#include <stdio.h>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int solution(vector<int> d, int budget) {
    int answer = 0;
    sort(d.begin(),d.end());
    for(auto it = d.begin(); it!=d.end(); ++it){
        if(budget>= *it) ++answer,budget -= *it;
        else break;
    }
    
    return answer;
}
```

-----

## 5. 결과

필요시.














 
