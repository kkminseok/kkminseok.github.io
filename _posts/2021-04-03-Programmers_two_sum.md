---
title: Programmers_두 정수 사이의 합
author: 강민석
date: 2021-04-03 12:12:12 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 두 정수 사이의 합 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12912>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

long long solution(int a, int b) {
    long long answer = 0;
    for(int i = min(a,b); i<=max(a,b);++i)
        answer += i;
    return answer;
}
```

-----

## 5. 결과

필요시.














 
