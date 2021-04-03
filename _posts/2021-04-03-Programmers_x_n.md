---
title: Programmers_x만큼 간격이 있는 n개의 숫자
author: 강민석
date: 2021-04-03 01:30:30 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - x만큼 간격이 있는 n개의 숫자 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12954>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 쉬운 문제입니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

vector<long long> solution(int x, int n) {
    vector<long long> answer;
    for(int i = 1 ; i<=n;++i)
        answer.push_back(x * i);
    return answer;
}
```

-----

## 5. 결과

필요시.














 
