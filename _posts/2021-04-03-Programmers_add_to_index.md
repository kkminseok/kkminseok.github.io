---
title: Programmers_자릿수 더하기
author: 강민석
date: 2021-04-03 15:15:15 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 자릿수 더하기 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12931>






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
#include <iostream>

using namespace std;
int solution(int n)
{
    int answer = 0;
    while(n!=0)
        answer += n%10, n/=10;
    return answer;
}
```

-----

## 5. 결과

필요시.














 
