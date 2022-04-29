---
title: Programmers_이진 변환 반복하기
author: 강민석
date: 2021-05-07 03:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -이진 변환 반복하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/70129>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌 1 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- Greedy하게 접근하면 어렵지 않은 문제입니다.





-----  

## 4. 접근 방법을 적용한 코드


```python
# 2진수로 바꾸는 함수
def makestr(onecount) -> str:
    str = ""
    while onecount != 0: 
        if onecount%2 == 0 :
            str += "0"
        else:
            str += "1"
        onecount//=2
    #역순으로 들어갔으므로 역순으로 다시 리턴
    return str[::-1]

def solution(s):
    answer = []
    # 삭제된 0의 갯수
    zerocount = 0
    #삭제시킨 횟수
    total = 0
    while s != "1" : 
        onecount = s.count('1')
        zerocount += (len(s) - onecount)
        s = makestr(onecount)
        total += 1
    answer.append(total)
    answer.append(zerocount)
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
