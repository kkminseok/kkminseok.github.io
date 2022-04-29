---
title: Programmers_연습문제 잃어버린 기록 찾기(SQL)
author: 강민석
date: 2021-07-02 18:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -잃어버린 기록 찾기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59042>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다.   

-----  

## 3. 생각한 것들(문제 접근 방법)

- JOIN

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT OUTS.ANIMAL_ID, OUTS.NAME 
FROM ANIMAL_OUTS OUTS LEFT JOIN ANIMAL_INS INS
ON OUTS.ANIMAL_ID = INS.ANIMAL_ID
WHERE INS.ANIMAL_ID IS NULL
ORDER BY OUTS.ANIMAL_ID;
```


-----



## 5. 결과















 
