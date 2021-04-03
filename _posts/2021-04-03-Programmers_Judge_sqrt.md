---
title: Programmers_정수 제곱근 판별
author: 강민석
date: 2021-04-03 07:12:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 정수 제곱근 판별 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12934>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.
- 계산이 부정확해서 틀렸다고 생각했는데 잘 넘어갔습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <cmath>

using namespace std;

long long solution(long long n) {
    long long answer = 0;
    int find = (sqrt(n) - (int)sqrt(n)) >0 ? -1 : sqrt(n);
    return find == -1 ? -1 : pow(find+1,2);
}
```

-----



## 5. 결과

필요시.














 
