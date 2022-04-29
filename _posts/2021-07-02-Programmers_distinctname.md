---
title: Programmers_연습문제 중복 제거하기(SQL)
author: 강민석
date: 2021-07-02 13:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -중복 제거하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59408>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다.   

-----  

## 3. 생각한 것들(문제 접근 방법)

- SQL
- GROUP BY가 아니라는 점.

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT COUNT(DISTINCT NAME) AS count 
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
```


-----



## 5. 결과















 
