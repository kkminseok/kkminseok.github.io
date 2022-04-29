---
title: leetcode(리트코드)-182 Duplicate Emails(SQL)
author: 강민석
date: 2021-06-30 05:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 182 - Duplicate Emails 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/duplicate-emails/> 

![](/assets/img/sample/leetcode/182/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/182/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 중복되는 Email을 찾아서 리턴합니다.


-----  

## 5. code  

코드설명

**MYSQL**

```sql
select Email
from Person
group by Email
having count(*) > 1
/*
-- 내가 작성한 것.
SELECT distinct p.Email 
FROM Person p, Person p2
WHERE p.Id <> p2.Id and p.Email = p2.Email 
*/
      
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/182/result.JPG)  

필요시 c++로 짜드리겠습니다.



