---
title: Programmers_최댓값과 최솟값
author: 강민석
date: 2021-05-05 05:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -최댓값과 최솟값 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12939>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다. 데이터도 크게 들어오지 않습니다.
- 문자열을 정수로 바꾸고 값을 찾은 다음 다시 정수를 문자열로 바꿨습니다.



-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(s):
    answer = ''
    temp = s.split(' ')
    min = int(temp[0])
    max = int(temp[0])
    for i in range(len(temp)):
        ele = int(temp[i])
        if min > ele:
            min = ele
        if max < ele:
            max = ele
    answer += str(min)
    answer += ' '
    answer += str(max)
    return answer
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
