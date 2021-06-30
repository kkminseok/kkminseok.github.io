---
title: leetcode(리트코드)-176 Second Highest Salary(SQL)
author: 강민석
date: 2021-06-30 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 176 - Second Higest Salary  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/second-highest-salary/> 

![](/assets/img/sample/leetcode/176/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/176/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 2번째로 큰 Salary를 뽑는 SQL문을 작성합니다.


-----  

## 5. code  

코드설명

**python**

```sql
-- 가장 큰 Salary보다 작은 값들 중에서 가장 큰 Salary를 뽑는 SQL입니다.
SELECT MAX(Salary) AS SecondHighestSalary 
FROM Employee
WHERE Salary < (SELECT MAX(Salary) FROM Employee);            
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/176/result.JPG)  

필요시 c++로 짜드리겠습니다.



