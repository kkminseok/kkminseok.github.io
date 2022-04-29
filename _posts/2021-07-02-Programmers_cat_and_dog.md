---
title: Programmers_연습문제 고양이와 개는 몇 마리 있을까(SQL)
author: 강민석
date: 2021-07-02 06:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,MYSQL]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -고양이와 개는 몇 마리 있을까 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/59040>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 2난이도의 문제입니다.   
롯데 pt면접에서 나왔던....


-----  

## 3. 생각한 것들(문제 접근 방법)

- SQL

-----  

## 4. 접근 방법을 적용한 코드

```sql
-- 코드를 입력하세요
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) AS count
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE ASC

```


-----



## 5. 결과















 
