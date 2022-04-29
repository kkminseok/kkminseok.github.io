---
title: Programmers_위클리챌린지 2주차 상호 평가(python)
author: 강민석
date: 2021-08-26 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 위클리 챌린지 2주차 - 상호 평가 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/83201>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
위클리 챌린지 2주차 문제입니다.

Level 1난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 어렵지 않습니다. 효율성을 고려하여 풀려했지만.. 전혀 만족스럽지 않은 코드가 탄생하였습니다.
- 


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(scores):
    row = len(scores)
    col = len(scores[0])
    answer = ""
    for i in range(col):
        countnum = [0] * 101 
        maxnum = 0
        minnum = 101
        sumnum = 0
        mine = 0
        res = 0
        for j in range(row) : 
            if i == j :
                mine = scores[j][i]
            maxnum = max(maxnum,scores[j][i])
            minnum = min(minnum,scores[j][i])
            sumnum += scores[j][i]
            countnum[scores[j][i]] += 1

        if (mine == minnum or mine == maxnum) and countnum[mine] == 1 : 
            res = (sumnum - mine) / (row-1)
        else : 
            res = sumnum / row
        plus = 10 
        if res != 100 :
            plus = int(10 - res // 10)
            if plus > 4 and plus < 6:
                plus = 4
            if plus > 5 : 
                plus = 6 
            answer +=  chr(plus-1 + ord("A"))
        else : 
            answer += "A"
        
    
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
