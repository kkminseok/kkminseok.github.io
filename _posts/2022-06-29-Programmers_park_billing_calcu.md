---
title: Programmers_2022 KAKAO BLIND RECRUITMENT 주차 요금 계산
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-06-29 02:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
comments : true
---


**프로그래머스 2022 KAKAO BLIND RECRUITMENT - 주차 요금 계산 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/92341>

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2022 KAKAO BLIND RECRUITMENT 문제 입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 구현 문제치고는 어렵지 않습니다. 
- 문자 함수들을 사용하지 않는게 더 편해보여서 그렇게 풀었습니다.


-----  

## 4. 접근 방법을 적용한 코드

```python

from collections import deque
from datetime import datetime,timedelta
def calcuBill(value,fees):
    if value < fees[0] :
        return fees[1]
    else:
        mod = (value-fees[0]) //fees[2]
        if (value-fees[0]) % fees[2] != 0:
            mod+=1
        return fees[1] + mod * fees[3]

def caclcuDifTime(in_time,out_time):
    return out_time - in_time
def dateFormat(time):
    hour,min = time.split(':')
    return int(hour) * 60 + int(min)
def solution(fees, records):
    answer = []
    dict = {}
    result = {}
    billing = deque()
    for info in records:
        time, carnum, status = info.split(' ')
        if carnum not in result:
            result[carnum] = 0
        time = dateFormat(time)
        # 2번째
        if carnum in dict:
            dif_t = caclcuDifTime(dict[carnum],time)
            del dict[carnum]
            result[carnum] += dif_t
            continue
        dict[carnum] =time
    for key,value in dict.items():
        result[key] += (1439 - value)
    sorted_dict = sorted(result.items())
    for key,value in sorted_dict:
        bill = calcuBill(value,fees)
        answer.append(bill)
	
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.

