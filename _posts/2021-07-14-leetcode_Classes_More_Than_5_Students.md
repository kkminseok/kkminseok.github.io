---
title: leetcode(리트코드)-596 Classes More Than 5 Students(SQL)
author: 강민석
date: 2021-07-14 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 596 - Classes More Than 5 Students  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/classes-more-than-5-students/> 

![](/assets/img/sample/leetcode/596/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/596/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- student 컬럼과 class 컬럼이 있습니다.
- class의 해당되는 student가 5명 이상인 것을 추출하는 쿼리를 작성하세요.
- 중복으로 값이 들어올 수 있습니다. 

-----  

## 5. code  

코드설명

**SQL**

```sql
# Write your MySQL query statement below
SELECT class
FROM courses
GROUP BY class
HAVING count(distinct student) >= 5
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/596/result.JPG)  





