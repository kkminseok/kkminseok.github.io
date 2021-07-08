---
title: leetcode(리트코드)-620 Not Boring Movies(sql)
author: 강민석
date: 2021-07-07 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 620 - Not Boring Movies 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/not-boring-movies/> 

![](/assets/img/sample/leetcode/620/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/620/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- id가 홀수이면서 description이 'boring'이 아닌 데이터를 rating의 내림차순으로 정렬하는 쿼리를 작성하세요.

-----  

## 5. code  

코드설명

**SQL**

```SQL
SELECT * 
FROM Cinema
WHERE id % 2 = 1 AND description NOT LIKE 'boring'
ORDER BY rating DESC;
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/620/result.JPG)  




