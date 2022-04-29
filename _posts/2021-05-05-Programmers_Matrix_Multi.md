---
title: Programmers_행렬의 곱셈
author: 강민석
date: 2021-05-05 11:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -행렬의 곱셈 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12949>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다. 데이터도 크게 들어오지 않습니다.
- greedy하게 풀었는데, 시간을 더 줄일 수 있는 방법이 있습니다.



-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(arr1, arr2):
    answer = []
    m = len(arr1)
    n = len(arr2[0])
    max = len(arr1[0])
    print(m,n)
    for i in range (0,m):
        answer.append([])
        for j in range(0,n):
            answer[i].append(0)
            for k in range(0,max):
                answer[i][j] += (arr1[i][k] * arr2[k][j])
    
    return answer
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
