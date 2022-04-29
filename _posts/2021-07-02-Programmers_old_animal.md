---
title: Programmers_연습문제 오랜 기간 보호한 동물(1)(SQL)
author: 강민석
date: 2021-07-02 20:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -오랜 기간 보호한 동물(1) 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59044>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다.   


-----  

## 3. 생각한 것들(문제 접근 방법)

- join

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT INS.NAME, INS.DATETIME
FROM ANIMAL_INS INS LEFT JOIN ANIMAL_OUTS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE OUTS.ANIMAL_ID IS NULL
ORDER BY INS.DATETIME
LIMIT 3
```


-----



## 5. 결과

















 
