---
title: Programmers_오픈 채팅방
author: 강민석
date: 2021-05-17 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -오픈 채팅방 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/42888#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2019 KaKao Blind Recuritment 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 채팅방의 로그를 찍어서 나중에 뿌려줍니다.
- Enter, Leave, Change 3가지 상태가 주어지는데, Enter와 Leave의 경우 로그에 찍으면 되고, Change인 경우 채팅방의 닉네임을 바꿔줘야합니다.
- 마지막의 로그들이 중요하기에(마지막에 이름을 바꾸는 게 중요하므로) record의 맨 끝에서 처음으로 접근하는 방식으로 코드를 작성하였으나, Runtime Error가 떠서 처음부터 접근하는 for문을 2번 작성하였습니다.






-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(record):
    # 정답
    answer = []
    dic = {}
    # 로그
    log =[]
    for i in range(len(record)):
        # " "를 기준으로 분리합니다.
        query = record[i].split(" ")
        # Leave가 아닌경우, dic에 중복값을 없애주기 위해 저장해줍니다.
        if(query[0] != "Leave") :
            dic[query[1]] = query[2]
        # Enter나 Leave인 경우 log list에 id와 심볼을 저장합니다.
        if(query[0] == "Enter") : 
            log.append((query[1],"E"))
        elif(query[0] =="Leave") : 
            log.append((query[1],"L"))
    # log를 돌면서 answer에 맞는 답안으로 바꿔줍니다.
    for i in range(len(log)):
        if log[i][1]=="E":
            answer.append(dic[log[i][0]] + "님이 들어왔습니다.")
        else:
            answer.append(dic[log[i][0]] + "님이 나갔습니다.")
    
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
