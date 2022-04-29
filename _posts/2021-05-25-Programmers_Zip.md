---
title: Programmers_2018 KAKAKO BLIND RECURITMENT[3차] 압축(python)
author: 강민석
date: 2021-05-25 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -압축 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17684>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
KAKAO BLIND RECURITMENT[3차] 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적으로 풀었습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(msg):
    answer = []
    dic = {}
    chat = "A"
    #ASCII 코드로 변환하면서 dictionary에 넣어줍니다.
    for i in range(26):
        ctoi = ord(chat)
        dic[chat] = i+1
        ctoi+=1
        chat = chr(ctoi)
    
    i=0
    #사전 접근 인덱스
    dicidx = 27
    while i < len(msg):
        dicchat = msg[i]
        answeridx = dic[dicchat]
        #다음 문자들을 포함한 것이 사전에 있는 지 확인합니다.
        while i+1 < len(msg) and dicchat + msg[i+1] in dic : 
            dicchat = dicchat + msg[i+1]
            answeridx =  dic[dicchat]
            i+=1
        
        #배열을 벗어나지 않게 사전에 없는 정보를 추가해줍니다.
        if i+1 < len(msg) : 
            dic[dicchat+ msg[i+1]] = dicidx
            dicidx+=1
        #결과값에 사전에서 찾은 값을 넣어줍니다.
        answer.append(answeridx)
        i+=1
            
    
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
