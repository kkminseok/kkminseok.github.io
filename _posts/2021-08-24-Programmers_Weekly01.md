---
title: Programmers_위클리챌린지 1주차 부족한 금액 계산하기(python)
author: 강민석
date: 2021-08-24 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 위클리 챌린지 1주차 - 부족한 금액 계산하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/82612>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
위클리 챌린지 1주차 문제입니다.

Level 1난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 어렵지 않습니다. 효율성을 고려해서 등차수열 합계산을 이용하여 풀었습니다.

- 


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(price, money, count):
    result = money - price * (count * (count + 1) // 2)
    return 0 if result >0 else abs(result)
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
