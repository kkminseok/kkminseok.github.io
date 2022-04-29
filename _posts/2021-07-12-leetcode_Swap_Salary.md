---
title: leetcode(리트코드)-627 Swap Salary(SQL)
author: 강민석
date: 2021-07-12 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 627 - Swap Salary  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/swap-salary/> 

![](/assets/img/sample/leetcode/627/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/627/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Salary 테이블에서 sex의 값이 'f'이면 'm'으로 바꾸고 'm'이면 'f'로 바꿔야합니다.

-----  

## 5. code  

코드설명

**SQL**

```sql
UPDATE Salary
SET sex = IF (sex = 'f','m','f')

        
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/627/result.JPG)  

필요시 c++로 짜드리겠습니다.



