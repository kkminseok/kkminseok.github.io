---
title: Programmers_위클리챌린지 4주차 직업군 추천하기(python)
author: 강민석
date: 2021-08-23 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 위클리 챌린지 4주차 - 직업군 추천하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/84325>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
위클리 챌린지 4주차 문제입니다.

Level 1난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 어렵지는 않지만 자료구조에 대해 여러가지 생각을 할 수 있게끔 도와주는 좋은 문제인 것 같습니다. 자료의 크기가 크지 않아서 푸는데 지장은 없지만 테이블의 크기가 1만개가 넘는다면 어떻게 최적화할 지 생각하여 코드를 작성하게 되었습니다. 

- 


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(table, languages, preference):
    answer = ''
    maxres = []
    maxnum = 0
    for i in range(len(table)):
        sumnum = 0
        # 공백을 기준으로 분리합니다. 점수를 확인하기 쉽게 인덱스로 나누었습니다.
        templist = table[i].split(' ')
        for j in range(len(languages)):
            score = 0
            try : 
                # 테이블이 5개만 들어오기에 6 - 찾은 인덱스를 해줍니다. 6인 이유는 처음에 SI, GAME 등 종목이 들어오기 때문입니다.
                score = 6 - (templist.index(languages[j]))
            except : 
                scroe = 0
            sumnum += score * preference[j]
        # 종목과 점수합계를 리스트에 넣습니다.
        maxres.append([templist[0],sumnum])       
    for i in range(len(maxres)):
        #사전순으로 업데이트 해줍니다.
        if maxnum == maxres[i][1] :
            answer = answer if answer < maxres[i][0] else maxres[i][0]
        elif maxnum < maxres[i][1] :
            answer = maxres[i][0]
            maxnum = maxres[i][1]
        
            
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
