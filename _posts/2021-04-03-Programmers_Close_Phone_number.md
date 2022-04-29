---
title: Programmers_휴대폰 번호 가리기
author: 강민석
date: 2021-04-03 11:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 휴대폰 번호 가리기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12948>






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

string solution(string phone_number) {
    string answer = "";
    for(int i = phone_number.size()-5; i>=0;--i)
        phone_number[i]='*';
    answer=phone_number;
    return answer;
}
```

-----

## 5. 결과

필요시.














 
