---
title: Programmers_연습문제 야근 지수(python)
author: 강민석
date: 2021-06-02 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -야근 지수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12927#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제는 직관적이고, n만큼 돌아도 문제가 풀릴 것이라 생각했습니다.
- Level3 답게 일반적인 방법으로는 안풀립니다.
- Priority Queue로 풀면 시간초과가 뜨고 heap으로 풀면 시간초과가 뜨지 않습니다.(python 기준)





-----  

## 4. 접근 방법을 적용한 코드

```python
#우선순위 큐로 푼 흔적
from queue import PriorityQueue
import heapq
def solution(n, works):
    answer = 0
    heap = []
    # 우선순위를 위해 -로 바꿔 넣습니다.
    for work in works :
        heapq.heappush(heap,-work)
    
    #n만큼 돌면서 우선순위를 갱신해줍니다.
    while n :
        work = -(heapq.heappop(heap))
        work-=1
        heapq.heappush(heap,-work)
        n-=1
    # -h가 음수인 경우는 고려할 필요가 없습니다.
    for h in heap:
        if -h < 0 :
            continue
        #어차피 - 곱하기 - 는 양수이므로 그냥 곱해버렸습니다.
        answer += h*h
    
    return answer

```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
