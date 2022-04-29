---
title: Programmers_연습문제 중성화 여부 파악하기(SQL)
author: 강민석
date: 2021-07-02 14:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -중성화 여부 파악하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59409>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다.   
LIKE Query가 보기에 안좋아서 다시 작성하고 싶은데..


-----  

## 3. 생각한 것들(문제 접근 방법)

- SQL
- IF

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT ANIMAL_ID, NAME, IF(SEX_UPON_INTAKE LIKE 'Neutered%' OR SEX_UPON_INTAKE LIKE 'Spayed%', 'O','X') AS "중성화"
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC;

```


-----



## 5. 결과















 
