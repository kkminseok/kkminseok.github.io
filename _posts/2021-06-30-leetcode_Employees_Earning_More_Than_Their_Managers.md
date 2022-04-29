---
title: leetcode(리트코드)-181 Employees Earning More Than Their Managers(SQL)
author: 강민석
date: 2021-06-30 04:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 181 - Employees Earning More Than Their Managers 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/employees-earning-more-than-their-managers/> 

![](/assets/img/sample/leetcode/181/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/181/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Employees 테이블에서 자신의 Manager보다 Salary를 많이 받는 사원의 이름을 검색해서 리턴합니다.


-----  

## 5. code  

코드설명

**MYSQL**

```sql
/*
SELECT e1.Name AS Employee
FROM Employee AS e1, Employee AS e2
WHERE e1.ManagerId = e2.Id and e1.Salary > e2.Salary;
*/

-- 조인을 사용
SELECT emp.Name Employee
FROM Employee emp INNER JOIN Employee manager
on emp.ManagerId = manager.Id
WHERE emp.Salary > manager.Salary;
          
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/181/result.JPG)  

필요시 c++로 짜드리겠습니다.



