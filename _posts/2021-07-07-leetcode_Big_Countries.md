---
title: leetcode(리트코드)-595 Big Countries(sql)
author: 강민석
date: 2021-07-07 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 595 - Big Countries 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/big-countries/> 

![](/assets/img/sample/leetcode/595/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/595/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- population이 25 million이 넘거나 area가 3 million이 넘는 것들의 name, population, area를 출력하는 쿼리를 작성하세요.

-----  

## 5. code  

코드설명

**SQL**

```SQL
SELECT name, population, area
FROM World
WHERE population > 25000000 or area > 3000000;

```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/595/result.JPG)  




