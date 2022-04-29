---
title: Programmers_JadenCase 문자열 만들기
author: 강민석
date: 2021-05-05 11:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -JadenCase 문자열 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12951>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다. 데이터도 크게 들어오지 않습니다.
- python의 capitalize()함수를 활용했습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(s):
    answer = s.split(' ')
    res = ''
    for i in range (len(answer)):
        answer[i] = answer[i].capitalize()
        res += answer[i]+ ' '
    #마지막에 공백이 추가되어있으므로
    return res[:-1]
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
