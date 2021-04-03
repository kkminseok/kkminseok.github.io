---
title: Programmers_하샤드 수
author: 강민석
date: 2021-04-03 11:12:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 하샤드 수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12947>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
using namespace std;

bool solution(int x) {
    int tempx = x;
    int sum = 0 ;
    while(tempx!=0){
        sum+=tempx%10;
        tempx/=10;
    }
    return x%sum ==0 ? true : false;
}
```

-----

## 5. 결과

필요시.














 
