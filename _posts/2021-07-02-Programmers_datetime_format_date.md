---
title: Programmers_연습문제 DATETIME에서 DATE로 형 변환(SQL)
author: 강민석
date: 2021-07-02 12:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -DATETIME에서 DATE로 형 변환 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59414>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다.   

-----  

## 3. 생각한 것들(문제 접근 방법)

- SQL
- date_format()이라는 함수를 알고 있었습니까?

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT ANIMAL_ID, NAME, date_format(DATETIME,'%Y-%m-%d') AS '날짜'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```


-----



## 5. 결과















 
