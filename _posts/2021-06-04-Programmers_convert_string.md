---
title: Programmers_연습문제 단어 변환(python)
author: 강민석
date: 2021-06-04 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -단어 변환 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/43163>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 어렵지 않은 DFS문제입니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
# 문자와 문자가 1개만큼 차이나는 지 확인하는 함수
def searchword(begin,target):
    counting = 0 
    for i in range(len(begin)):
        if begin[i] != target[i]:
            counting+=1
            if counting >1 : 
                return False
    return True
def DFS(begin,target,words,v,count,res) : 
    if begin == target : 
        res.append(count)
    for i in range(len(words)):
        #방문한 적이 없고 문자와 문자 차이가 1일 때
        if v[i] == 0 and searchword(begin,words[i]) : 
            v[i] = 1
            DFS(words[i],target,words,v,count+1,res)
            v[i] = 0
    
def solution(begin, target, words):
    #DFS
    #방문 처리
    v= [0] * len(words) 
    res = []
    DFS(begin,target,words,v,0,res)
    # 리스트가 비어있으면 0 리턴
    return min(res) if res else 0
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
