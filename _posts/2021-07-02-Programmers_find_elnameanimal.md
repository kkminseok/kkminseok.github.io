---
title: Programmers_연습문제 이름에 el이 들어가는 동물 찾기(SQL)
author: 강민석
date: 2021-07-02 09:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -이름에 el이 들어가는 동물 찾기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59047>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다.   

-----  

## 3. 생각한 것들(문제 접근 방법)

- SQL
- LIKE

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'Dog' AND NAME LIKE '%el%'
ORDER BY NAME
```


-----



## 5. 결과















 
