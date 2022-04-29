---
title: leetcode(리트코드)-175 Combine Two Tables(SQL)
author: 강민석
date: 2021-06-23 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 175 - Combine Two Tables 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/combine-two-tables/> 

![](/assets/img/sample/leetcode/175/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/175/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 두 테이블이 주어졌습니다. 
- FirstName과 LastName은 Person 테이블에 있습니다.
- City, State는 Address 테이블에 있습니다.
- 위의 4개를 select하는 구문을 작성합니다.





-----  

## 5. code  

코드설명

**SQL**

```sql
select Person.FirstName, Person.LastName, Address.City, Address.State 
from Person left join Address
on Person.PersonId =Address.PersonId

                
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/175/result.JPG)  





