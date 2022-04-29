---
title: Programmers_연습문제 동명 동물 수 찾기(SQL)
author: 강민석
date: 2021-07-02 10:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -이름에 el이 들어가는 동물 찾기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59041>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다.   

-----  

## 3. 생각한 것들(문제 접근 방법)

- SQL
- GROUP, HAVING

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT NAME, COUNT(NAME) AS COUNT
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME) >=2
ORDER BY NAME ASC
```


-----



## 5. 결과















 
