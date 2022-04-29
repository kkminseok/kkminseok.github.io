---
title: Programmers_2020 카카오 기출 괄호 변환(python)
author: 강민석
date: 2021-05-20 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -괄호 변환 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/60058>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2020 KaKao BLIND RECRUITMENT 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 재귀함수를 잘 구현할 수 있는 지 묻는 문제입니다.
- 과정을 잘 따라가면 어렵지 않습니다.
- checkstr() 함수는 '올바른 괄호인지 확인하는 함수입니다.'
- makestr() 함수는 '균형잡힌 괄호 2개로 나눠주는 함수입니다.'
- reversestr()함수는 '괄호들의 방향을 바꿔주는 함수입니다.'
- solv()함수는 기본적인 재귀함수입니다.

-----  

## 4. 접근 방법을 적용한 코드


```python

def checkstr(p):
    li = []
    # stack을 통해 p를 돌면서 괄호의 짝을 확인합니다.
    for i in range(len(p)):
        if p[i] == "(":
            li.append("(")
        else :
            if len(li) == 0 :
                return False
            li.pop()
    return True
def makestr(p):
    idx = 0
    left = 0 
    right = 0
    # p를 돌면서 '(' 갯수와 ')' 갯수가 맞는 최소 구간을 확인합니다.
    while idx <len(p):
        if p[idx] == "(" : 
            left += 1
        else :
            right += 1 
        if left == right : 
            break
        idx+=1
    return p[:left+right] , p[left+right : ]
def reversestr(p) : 
    temp = ""
    for i in range(len(p)):
        if p[i] =="(":
            temp+=")"
        else : 
            temp+="("
    return temp
        
def solv(p):
    if p =="":
        return ""
    if checkstr(p):
        return p
    u,v = makestr(p)
    #print("u : ", u,"v : ",v)
    if checkstr(u) :
        resv = u + solv(v)
        return resv
    else : 
        temp = "("
        temp += solv(v)
        temp += ")"
        u = u[1:-1]
        u = reversestr(u)
        temp+= u 
        return temp
        
    
    
def solution(p):
    p = solv(p)
    
    return p
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
