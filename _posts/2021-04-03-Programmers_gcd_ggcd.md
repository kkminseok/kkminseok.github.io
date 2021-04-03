---
title: Programmers_최대 공약수와 최소 공배수
author: 강민석
date: 2021-04-03 11:11:10 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 최대 공약수와 최소 공배수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12940>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.
- 유클리드 호제법을 사용하였습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

int gcd(int a, int b) {
    int r;
    while (b != 0) {
        r = a % b;
        a = b;
        b = r; 
    } 
    return a; 
}


vector<int> solution(int n, int m) {
    vector<int> answer;
    answer.push_back(gcd(n,m));
    answer.push_back((n*m) / gcd(n,m));
    return answer;
}
```

-----

## 5. 결과

필요시.














 
