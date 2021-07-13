---
title: leetcode(리트코드)-1179 Reformat Department Table(SQL)
author: 강민석
date: 2021-07-13 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1179 - Reformat Department Table  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reformat-department-table/> 

![](/assets/img/sample/leetcode/1179/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1179/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- id에 맞는 month들의 revenue합을 구하는 쿼리를 작성하세요.

-----  

## 5. code  

코드설명

**SQL**

```sql
# Write your MySQL query statement below
SELECT id, 
SUM(CASE WHEN month = 'Jan' THEN revenue ELSE NULL END) as Jan_Revenue,
SUM(CASE WHEN month = 'Feb' THEN revenue ELSE NULL END) as Feb_Revenue,
SUM(CASE WHEN month = 'Mar' THEN revenue ELSE NULL END) as Mar_Revenue,
SUM(CASE WHEN month = 'Apr' THEN revenue ELSE NULL END) as Apr_Revenue,
SUM(CASE WHEN month = 'May' THEN revenue ELSE NULL END) as May_Revenue,
SUM(CASE WHEN month = 'Jun' THEN revenue ELSE NULL END) as Jun_Revenue,
SUM(CASE WHEN month = 'Jul' THEN revenue ELSE NULL END) as Jul_Revenue,
SUM(CASE WHEN month = 'Aug' THEN revenue ELSE NULL END) as Aug_Revenue,
SUM(CASE WHEN month = 'Sep' THEN revenue ELSE NULL END) as Sep_Revenue,
SUM(CASE WHEN month = 'Oct' THEN revenue ELSE NULL END) as Oct_Revenue,
SUM(CASE WHEN month = 'Nov' THEN revenue ELSE NULL END) as Nov_Revenue,
SUM(CASE WHEN month = 'Dec' THEN revenue ELSE NULL END) as Dec_Revenue
FROM Department
GROUP BY id
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1179/result.JPG)  





