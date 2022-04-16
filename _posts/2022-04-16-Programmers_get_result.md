---
title: Programmers_2022 KAKAO BLIND RECRUITMENT
 신고 결과 받기(python)
author: 강민석
date: 2022-04-16 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 카카오 블라인드 2022 - 신고 결과 받기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/86051>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
프로그래머스 카카오 블라인드 2022 문제입니다.

Level 1난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 중첩 dictionary를 사용하였습니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(id_list, report, k):
    dict ={}
    result =[0] * len(id_list)
    for id in id_list :
        tmpdic ={}
        tmpdic['data'] = list()
        tmpdic['count'] = 0
        dict[id] = tmpdic
    for repdata in report : 
        fromp,to = repdata.split(' ')
        if to not in dict[fromp]['data'] :
            dict[fromp]['data'].append(to)
            dict[to]['count'] +=1
    idx = 0
    for data in dict :
        for id in dict[data]['data'] : 
            if dict[id]['count'] >= k :
                result[idx] +=1
        idx+=1
            
    return result
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
